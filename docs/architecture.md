# TogetherLog — Architecture

This document describes the high-level architecture of TogetherLog, including components, data flows, and major technical decisions.

---

## 1. High-Level Overview

TogetherLog consists of three main parts:

1. **Flutter App (Web + Android)**
   - UI layer: shows logs, entries, and the flipbook
   - Uses REST APIs to communicate with the backend
   - Implements navigation, state management, and rendering

2. **Supabase Backend**
   - PostgreSQL database
   - Authentication (email/password + Google OAuth)
   - Storage for photos and thumbnails (Supabase Storage, EU region)
   - Edge Functions (TypeScript/Deno) for:
     - REST API endpoints
     - Async background processing

3. **External Services**
   - OpenStreetMap/Nominatim for reverse geocoding

The goal is to keep the Flutter client as lean as possible and move non-trivial logic (Smart Pages, image processing, metadata extraction) to the backend.

---

## 2. Components

### 2.1 Flutter App

**Responsibilities:**

- Present the user interface (Web + Android)
- Manage navigation using `go_router`
- Manage state using `flutter_riverpod`
- Handle:
  - Authentication flows (using Supabase Auth)
  - Creating and editing logs
  - Uploading photos (through Supabase APIs / signed URLs)
  - Creating and editing entries (date, tags, highlight text, location)
  - Rendering Smart Page layouts and color themes returned by the backend
  - Displaying the flipbook with a 3D-like page-turn animation
  - Applying dynamic, per-page theming based on Smart Page data

**Non-responsibilities:**

- EXIF extraction
- Thumbnail creation
- Dominant color analysis
- Smart Page rule computation
- Storing data locally (beyond transient in-memory state)

The app is **online-only** in V1.

---

### 2.2 Supabase Backend

The Supabase project acts as the main backend and includes:

#### 2.2.1 PostgreSQL

PostgreSQL is the central, relational data store.

Conceptual tables include:

- `users`
- `logs`
- `entries`
- `photos`
- `tags`
- `entry_tags`
- Optional: `color_themes`, `layout_templates`, etc.

All data related to users, logs, and entries lives here.

#### 2.2.2 Authentication (Supabase Auth)

Supabase Auth handles:

- Email + password registration and login
- Google OAuth login
- Token-based authentication for the Flutter app

The Flutter app uses Supabase client libraries to authenticate and attach tokens to API requests.

#### 2.2.3 Storage (Supabase Storage)

Supabase Storage is used for:

- Original photos
- Generated thumbnails

Configuration:

- Hosted in an EU region (e.g., `eu-central-1`) for GDPR/DSGVO compliance
- Bucket(s) for `photos` and `thumbnails`
- Access controlled via policies (e.g., users can only access files belonging to their logs)

In the future, storage can be migrated to a self-hosted or alternative S3-compatible provider (e.g., Hetzner), with minimal logical changes to the application.

#### 2.2.4 Edge Functions (TypeScript / Deno)

Supabase Edge Functions are used for:

1. **REST API Endpoints**
   - Expose a REST API to the Flutter app
   - Example actions:
     - Create/read/update/delete logs
     - Create/read/update entries
     - Initiate file uploads (signed URLs or direct upload flow)
     - Trigger Smart Page recomputation for an entry

2. **Async Worker Functions**
   - Triggered after a photo is uploaded or an entry is created/updated
   - Responsibilities:
     - Extract EXIF metadata (date, GPS)
     - Generate thumbnails
     - Compute dominant colors for images
     - Call reverse geocoding (OpenStreetMap/Nominatim) when GPS is available
     - Compute Smart Page fields:
       - `page_layout_type`
       - `color_theme`
       - optional `sprinkles[]`
   - Update relevant database records with processed data

This approach keeps heavy or sensitive processing off the client and in an environment controlled by the backend.

---

### 2.3 External Services

#### 2.3.1 OpenStreetMap / Nominatim

Used for reverse geocoding:

- Input: GPS coordinates (lat, lng)
- Output: human-readable location (e.g., city, region, country)

Architecture decisions:

- Use public Nominatim for V1
- Implement **caching** on the backend to reduce API calls and respect rate limits
- Optionally switch to a commercial or self-hosted instance later

---

## 3. Data Flow

### 3.1 Creating a New Entry

1. **User Action**
   - User selects a log and chooses to create a new entry.
   - User selects one or more photos in the Flutter app.

2. **Upload**
   - Flutter app requests an upload operation (via a REST endpoint or Supabase client).
   - Photos are uploaded to Supabase Storage.
   - Photo metadata (e.g., `photo_id`, `url`) is stored in PostgreSQL.

3. **Entry Creation**
   - Flutter app sends a request to create an entry:
     - References the uploaded photos
     - Includes initial user data:
       - `event_date` (if known or from EXIF if already available)
       - `highlight_text`
       - `tags[]`
       - optional manual location

4. **Async Processing**
   - A worker Edge Function is triggered (e.g., via database trigger, storage event, or explicit API call).
   - Worker:
     - Extracts EXIF data
     - Updates `event_date` if not set
     - Extracts GPS and calls Nominatim to get `display_name`
     - Generates thumbnails and stores them in storage
     - Computes dominant colors
     - Runs deterministic Smart Page rules and sets:
       - `page_layout_type`
       - `color_theme`
       - optional `sprinkles[]`
     - Saves all results back to PostgreSQL

5. **Client Refresh**
   - The Flutter app:
     - Either polls or receives updated entry data (via reload or re-fetch)
     - Renders the entry as part of the flipbook using the Smart Page configuration

---

### 3.2 Viewing the Flipbook

1. User selects a log in the Flutter app.
2. Flutter app fetches entries from the REST API (sorted chronologically).
3. Each entry is mapped to a two-page spread using:
   - `page_layout_type`
   - `color_theme`
   - `photos[]`
   - `highlight_text`
   - `location`
4. Flipbook component uses a simple 3D page-turn animation to move between spreads.

---

## 4. API Design (High Level)

The API is **REST-based** and implemented via Supabase Edge Functions.

Example endpoint categories:

- `/logs`
  - `GET /logs` – list logs for current user
  - `POST /logs` – create new log
  - `GET /logs/:id` – get a specific log
  - `PATCH /logs/:id` – update log
  - `DELETE /logs/:id` – delete log

- `/entries`
  - `GET /logs/:logId/entries` – list entries for a log
  - `POST /logs/:logId/entries` – create new entry
  - `GET /entries/:id` – get entry details
  - `PATCH /entries/:id` – update entry (tags, text, location override)
  - `DELETE /entries/:id` – delete entry

- `/uploads`
  - Endpoints (or Supabase client usage) for initiating or handling file uploads

The exact URL structure and payload formats can be refined in a separate API spec.

---

## 5. Frontend Architecture (Flutter)

### 5.1 Structure

Basic structure under `/app/lib`:

- `features/`
  - `auth/` – login, signup, Google OAuth integration
  - `logs/` – log list, create/edit log
  - `entries/` – create/edit entries, entry detail
  - `flipbook/` – flipbook viewer, page-turn animation
- `core/`
  - `theme/` – global theme configuration, dynamic page colors
  - `routing/` – `go_router` setup and route definitions
  - `widgets/` – reusable UI components
- `data/`
  - `api/` – Supabase + Edge Functions clients, REST calls
  - `models/` – Dart models for logs, entries, photos, tags, layouts

### 5.2 State Management

- `flutter_riverpod` for:
  - Auth state
  - Current log/entry state
  - API data fetching and caching in memory
  - Flipbook navigation state

---

## 6. Deployment Strategy

### 6.1 V1 Approach

- **Manual deployments**:
  - Flutter Web: build and upload to static hosting
  - Flutter Android: build APK/AAB and distribute or publish
  - Supabase:
    - Edge Functions deployed via Supabase CLI
    - SQL migrations applied using Supabase tooling

CI/CD via GitHub Actions is not required for V1 but the repository structure is prepared for adding it later.

---

## 7. Open Source & Self-Hosting

The project is intended to use the **MIT license**.

Long-term goals:

- Allow others to:
  - Clone the repository
  - Spin up their own Supabase instance or a fully self-hosted alternative
  - Configure storage and geocoding providers
- Provide:
  - Contribution guidelines
  - Issue templates
  - Documentation for local development and deployment

---

## 8. Summary

Key architectural characteristics:

- **Flutter Web + Android** frontend
- **Supabase** backend (PostgreSQL, Auth, Storage, Edge Functions)
- **REST API** implemented in TypeScript (Deno) via Edge Functions
- **Async workers** for media processing and Smart Page computation
- **Deterministic Smart Pages** and Emotion-Color-Engine V1 on the backend
- **OpenStreetMap/Nominatim** for geocoding with caching
- Monorepo structure optimized for open source and future self-hosting

This architecture balances:
- Simplicity for a solo developer
- A clean separation of responsibilities
- A clear path from managed to self-hosted deployments