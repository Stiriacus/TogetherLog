# TogetherLog

TogetherLog is a modern, elegant platform for couples and close groups to document shared memories as an interactive online flipbook.

Built with Flutter (Web + Android) on the frontend and Supabase (PostgreSQL, Auth, Storage, Edge Functions) on the backend, TogetherLog aims to be open-source-friendly and eventually self-hostable while starting with a pragmatic managed setup.

---

## ✨ Features (Version 1 – MVP)

Platforms
• Flutter Web
• Flutter Android

Accounts & Auth
• Email + password
• Google OAuth via Supabase Auth

Logs (Memory Books)
• Multiple logs per user
• Each log is a chronological digital flipbook

Entries (Memories)
• Upload 1–N photos
• EXIF date extraction
• EXIF GPS + reverse geocoding via OpenStreetMap/Nominatim
• Editable/overrideable location
• Tag system with predefined categories
• Short highlight text

Smart Pages (Backend-Driven)
• Fully deterministic, rule-based engine
• Backend selects:
– page layout type
– color theme (Emotion-Color-Engine V1)
• Flutter client only renders the given layout

Flipbook Viewer
• Simple 3D-like page-turn animation
• Each entry displayed as a two-page spread
• Works well on Web + Android

Processing Pipeline
• Async worker via Supabase Edge Functions
• Handles:
– EXIF extraction
– thumbnail generation
– dominant color computation
– Smart Page evaluation
• Online-only (no offline mode in V1)

---

## 🔭 Future Features (Planned for V2+)

See docs/v2optional.md for details.
Highlights:

• Map view (OpenStreetMap-based)
• Story slideshow
• Sprinkles (decorative icons), polaroid & collage layouts
• Optional AI assistance (tagging, layout suggestions)
• Relationship/group progress (opt-in)
• Heatmaps of visited places
• Mobile widgets for random memories

---

## 🧱 Project Structure (Monorepo)

/app
/lib
/features
/auth
/logs
/entries
/flipbook
/core
/theme
/routing
/widgets
/data
/api
/models
pubspec.yaml

/backend
/edge-functions
/api # REST endpoints (Supabase Edge Functions)
/workers # async workers (EXIF, thumbnails, colors)
/sql
/migrations # PostgreSQL migrations (Supabase)
README.md

/docs
v1Spez.md
v2optional.md
architecture.md

/.github
/workflows
ci.yml

LICENSE
README.md

---

## 🛠 Tech Stack

Frontend (Flutter)
• Flutter Web + Flutter Android
• go_router for navigation
• flutter_riverpod for state management
• dio or http for REST communication
• Simple 3D page-turn animation package
• Light theme only for now; dynamic per-page theming driven by Smart Pages

Backend (Supabase)
• Supabase PostgreSQL
• Supabase Auth (email/password + Google)
• Supabase Storage (EU region) for photos + thumbnails
• Supabase Edge Functions (TypeScript/Deno) for:
– REST API
– async workers
– Smart Page computation

Geocoding
• OpenStreetMap / Nominatim
• Public Nominatim with backend caching

---

## 🚀 Deployment

V1 deployment is manual:

Frontend
• Flutter Web: manual build + upload to static host
• Android: manual APK/AAB build

Backend
• Supabase project hosts the backend
• Migrations + Edge Functions deployed via Supabase CLI

Automatic CI/CD can be added later using GitHub Actions.

---

## 📚 Documentation

docs/v1Spez.md – MVP technical specification
docs/v2optional.md – optional features
docs/architecture.md – architectural overview

---

## 🤝 Contributing

TogetherLog is intended to be open-source-friendly.

Repository will include:
• MIT license
• Contribution guidelines
• Issue + PR templates
• Clear folder structure

Contributions welcome for:
• UI/UX (Flutter)
• Smart Page layout ideas
• Supabase backend enhancements
• Documentation

---

## 📄 License

MIT.
Ensure LICENSE file is present before making the repo public.

---

## ❤️ Goal

TogetherLog aims to make shared memories:

• Beautiful to look at
• Simple to capture
• Respectful of privacy
• Easy to self-host

While staying lightweight, clean, and architecturally maintainable.

---