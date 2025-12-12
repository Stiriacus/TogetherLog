# TogetherLog — Design Theme Guide (V1.1)

**Status:** Approved Direction
**Version:** 1.1 (with Gap Resolution Addendum)
**Scope:** Application Chrome & UI System
**Excludes:** Smart Pages, Flipbook Internal UI
**Platforms:** Flutter Web (primary), Flutter Android (secondary)

---

## 1. Purpose

This document defines the **global design theme** for TogetherLog.

Its purpose is to:
- Establish a consistent visual identity for the app shell
- Ensure UI elements never compete with memory content
- Provide a clear, implementable reference for Flutter developers
- Act as a stable design contract alongside backend-driven Smart Pages

This theme applies to **all UI outside Smart Page rendering**.

---

## 2. Design Philosophy

### Core Principles

- **Content-first:** Memories are the product, UI is the frame.
- **Warm & cozy:** Emotional, calm, human.
- **Journal-inspired:** Subtle scrapbook feeling, not decorative.
- **Minimal branding:** Brand presence is intentionally quiet.
- **Acceptable contrast:** Smart Pages may clash; this is allowed.

### Mental Model

> The application chrome is a museum wall.  
> Smart Pages are the artwork.

---

## 3. Color System

### 3.1 Core Palette

| Token | HEX | Usage |
|------|-----|------|
| carbonBlack | #1D1E20 | Primary text, anchors |
| darkWalnut | #592611 | Primary accent, CTAs |
| oliveWood | #785D3A | Secondary accents, borders |
| softApricot | #FCDCB5 | Highlights, warm surfaces |
| antiqueWhite | #FAEBD5 | App background, cards |

---

### 3.2 Functional Color Roles

#### Backgrounds
- App background: Antique White
- Cards / sheets: Antique White
- Elevated highlights: Soft Apricot (sparingly)

#### Text
- Primary: Carbon Black
- Secondary: Olive Wood (80%)
- Disabled / hint: Olive Wood (50%)

#### Actions
- Primary action: Dark Walnut
- Secondary action: Olive Wood
- Hover / pressed overlay: Soft Apricot (low opacity)

#### Status Colors (Neutral, non-brand)

| State | Token | HEX | Usage |
|-------|-------|-----|-------|
| Success | successMuted | #6F8F7A | Processing complete, Smart Page ready |
| Info | infoMuted | #6F7F8F | Informational banners, notices |
| Error | errorMuted | #9A5A5A | Validation errors, destructive warnings |

**Rules:**
- Never used as backgrounds for large surfaces
- Used for icons, borders, small banners only
- Text on these colors must remain Carbon Black or Antique White depending on contrast
- Status colors must never dominate the UI

---

## 4. Typography

### 4.1 Font Families (Google Fonts)

#### Heading Font
**Playfair Display**

Used for:
- Screen titles
- Empty state headlines
- Emotional emphasis outside Smart Pages

Purpose:
- Editorial, journal-like warmth

#### Body Font
**Inter**

Used for:
- All body text
- Forms and labels
- Navigation
- Metadata (dates, locations)

---

### 4.2 Type Scale

| Role | Size | Weight | Font |
|----|----|----|----|
| Display / Hero | 32–36 | SemiBold | Playfair |
| Screen Title | 22–24 | SemiBold | Playfair |
| Section Header | 18 | Medium | Inter |
| Body Primary | 14–16 | Regular | Inter |
| Body Secondary | 13–14 | Regular | Inter |
| Caption / Meta | 12 | Regular | Inter |
| Button Label | 14–16 | Medium | Inter |

Line height should remain generous (1.4–1.6).

---

## 5. Spacing & Layout

### 5.1 Base System
- 8dp spacing system

### 5.2 Common Spacing
- Small: 8
- Standard: 16
- Section: 24
- Major: 32

### 5.3 Page Padding
- Mobile: 16
- Desktop: 24–32

---

## 6. Shape & Elevation

### 6.1 Border Radius

| Element | Radius |
|------|------|
| Buttons | 8 |
| Inputs | 8 |
| Cards | 12 |
| Thumbnails | 12–16 |
| Chips | 16 |

### 6.2 Elevation Rules

Elevation is functional, never decorative.

| Element | Elevation |
|------|------|
| AppBar | 0–1 |
| Card | 1–2 |
| Dialog | 4 |
| BottomSheet | 6 |

#### Shadow Definition

**Base shadow color:** `rgba(120, 93, 58, 0.18)` (Olive Wood @ 18% opacity)

| Elevation | Offset (x,y) | Blur | Spread |
|-----------|--------------|------|--------|
| 1 | 0, 1 | 2 | 0 |
| 2 | 0, 2 | 4 | 0 |
| 4 | 0, 4 | 8 | -1 |
| 6 | 0, 6 | 12 | -2 |

**Rules:**
- Never stack multiple shadows
- No cool gray or black shadows allowed
- Flipbook shadows remain excluded

---

## 7. Component Styling

### AppBar
- Background: Antique White
- Text/icons: Carbon Black
- Minimal elevation
- Uses Inter, not Playfair

---

### Buttons

#### Primary
- Background: Dark Walnut
- Text: Antique White
- Rounded corners (8)
- Soft press animation

#### Secondary
- Border: Olive Wood
- Text: Olive Wood
- Hover background: Soft Apricot

#### Text Button
- Text: Dark Walnut
- Hover: underline or warm tint

---

### Floating Action Button
- Background: Dark Walnut
- Icon/Text: Antique White
- Extended FAB preferred
- Slight elevation for discoverability

---

### Cards
- Background: Antique White
- Radius: 12
- Minimal shadow
- No decorative borders

---

### Inputs
- Background: Antique White
- Border: Olive Wood (30%)
- Focus border: Dark Walnut
- Error state: muted red

---

### Chips / Tags
- Background: Soft Apricot
- Text: Carbon Black
- Selected: Dark Walnut background + Antique White text

---

### Interaction States

#### Hover / Pressed / Focus Opacity

| State | Overlay Color | Alpha |
|-------|---------------|-------|
| Hover | Soft Apricot | 0.08 |
| Pressed | Soft Apricot | 0.12 |
| Focus (background) | Soft Apricot | 0.10 |

**Rationale:** These values are visible without overpowering photography. Matches Material accessibility expectations while staying warm.

---

### Disabled States

#### Disabled Buttons
- Background: Olive Wood @ 20%
- Text/Icon: Olive Wood @ 50%
- No shadow
- No hover/press feedback

#### Disabled Inputs
- Background: Antique White
- Border: Olive Wood @ 20%
- Text: Olive Wood @ 50%
- Cursor disabled

#### Disabled Chips
- Background: Soft Apricot @ 40%
- Text: Olive Wood @ 50%
- No interaction

---

### Focus Ring (Accessibility)

| Property | Value |
|----------|-------|
| Outline color | Dark Walnut |
| Thickness | 2dp |
| Offset | 2dp outside component |
| Border radius | Matches component radius |

**Applies to:** Buttons, Inputs, FAB, Interactive cards (keyboard focus)

---

### Dividers

| Property | Value |
|----------|-------|
| Color | Olive Wood @ 20% |
| Thickness | 1dp |
| Spacing | 8–16dp vertical |

Used for section separation and list item grouping. Never decorative.

---

### Placeholder & Hint Text

| Element | Color |
|---------|-------|
| Placeholder text | Olive Wood @ 50% |
| Hint / helper text | Olive Wood @ 60% |

**Purpose:** Clearly secondary, never mistaken for actual content.

---

## 8. Icons

### 8.1 Size Specifications

| Context | Size |
|---------|------|
| Default icon | 24dp |
| Small icons (inline/meta) | 20dp |
| Large empty-state icons | 48–64dp |

### 8.2 Color Rules

- Icons inherit text color by default
- Active icons: Dark Walnut
- Inactive icons: Olive Wood @ 70%
- Disabled icons: Olive Wood @ 40%
- Empty state icons: Olive Wood @ 40%

**No multicolor icons outside Smart Pages.**

### 8.3 Log Type Icon Colors

Used only for log identification icons (never for UI chrome).

| Log Type | HEX | Description |
|----------|-----|-------------|
| Couple | #B86A7C | Muted rose |
| Friends | #6F86A8 | Muted blue |
| Family | #8A6FA8 | Muted purple |

**Rules:**
- Icons only, never backgrounds
- Do not reuse elsewhere
- Must not clash with Smart Pages (icons are small by design)

---

## 9. Motion & Interaction

### 9.1 Principles
- Soft, organic, never mechanical
- Emotionally supportive, not attention-seeking

### 9.2 Animation Timing

| Interaction | Duration |
|-------------|----------|
| Button press | 120ms |
| Hover transition | 150ms |
| Page transition | 220ms |
| Bottom sheet enter | 280ms |
| Dialog enter | 240ms |

### 9.3 Animation Curves

| Context | Curve |
|---------|-------|
| Default | easeOutCubic |
| Enter animations | easeOut |
| Exit animations | easeIn |

### 9.4 Usage Examples
- Page transitions: fade + slight vertical motion
- Buttons: micro-scale + color fade
- Lists: gentle fade-in
- Bottom sheets: slow, smooth rise

**Flipbook page-turn animation is excluded and isolated.**

---

## 10. Platform Adaptation

### Web (Primary)
- Wider max-width containers
- Hover states enabled
- More white space

### Android
- Slightly tighter spacing
- Clear touch targets (minimum 48dp)
- Muted ripple effects allowed

Visual identity must remain consistent.

---

## 11. Brand Presence

- Brand visuals are minimal
- No persistent logos after authentication
- Brand appears primarily on:
  - Auth screen
  - Logs list AppBar

---

## 12. Implementation Clarifications

These clarifications resolve ambiguities for implementation:

| Question | Answer |
|----------|--------|
| AppBar title alignment | Left-aligned (centered only on Auth screen) |
| FAB position | Bottom-right on Logs List and Entries List only |
| Card tap feedback | Soft Apricot overlay @ 0.08 alpha |
| Photo loading placeholder | Solid Antique White background (no shimmer) |
| Empty state icon color | Olive Wood @ 40% |

**Note:** Skeleton shimmer effects are explicitly discouraged.

---

## 13. Explicit Exclusions

This theme does NOT apply to:
- Smart Page layouts
- Flipbook internal UI
- Emotion-Color-Engine output
- Photo overlays

Smart Pages are allowed to clash visually.

---

## 14. Authority

This document is the **single source of truth** for TogetherLog UI theming.

Any future UI changes must:
- Respect this guide
- Or explicitly revise this document
