# Claude Code Briefing — Kismet House Tracker

Read this before starting any work on this project.

---

## What This Is

**Kismet House** is a family achievement tracker PWA for a beach house in Kismet, Fire Island, NY. Nine family members track badges, fish caught, and shared experiences in real time. It's a Progressive Web App — installable on phones, works offline, syncs live across all devices via Firebase.

This is also a **proof of concept** for a larger product idea: a location-based personal fulfillment and achievement tracker that works in any city. The Kismet house is the internal beta this summer. After summer, the concept expands.

---

## Technical Stack

- **Vanilla HTML/CSS/JS** — no React, no build step, no npm required
- **Firebase v10** via CDN (not npm)
- **Netlify** for hosting (auto-deploys from GitHub `main`)
- **PWA** — installable on iPhone/Android, works offline via service worker

---

## File Structure

```
kismet/
├── src/                    ← deployed to Netlify (publish directory: src)
│   ├── index.html          ← entire app (~1200 lines, HTML + CSS + JS)
│   ├── manifest.json       ← PWA manifest
│   ├── sw.js               ← service worker (offline support)
│   ├── icon-192.png
│   └── icon-512.png
├── content/
│   └── badges/             ← badge content system (see below)
│       ├── universal.md
│       ├── nyc.md
│       ├── fire_island.md
│       ├── neighborhood_templates.md
│       └── voice_guide.md
├── docs/                   ← product documents
├── CLAUDE_CODE_BRIEFING.md ← this file
├── README.md
└── .gitignore
```

---

## Deploy Pipeline

**Edit → commit → push → live in ~30 seconds.**

```bash
git add .
git commit -m "Description of what changed"
git push
```

Netlify auto-deploys on every push to `main`. Publish directory is `src`.

Live URL: **kismet-tracker.netlify.app**
GitHub: **github.com/pgfischer11/kismet-tracker**

---

## What NOT to Touch

- **Firebase credentials** in `initFirebase()` inside `index.html` — do not move, restructure, or remove
- **`LOCAL_SK = 'kismet_v5'`** — localStorage key for app state; changing this loses all local data
- **`WHO_SK = 'kismet_who_v1'`** — localStorage key for user identity; changing this forgets who's logged in
- **The 5-file structure in `src/`** — all files must stay together; Netlify serves from this folder

---

## App Architecture

All app logic lives in a single `<script>` block at the bottom of `index.html`.

**View routing:** `showView(name)` switches between:
- `home` — family grid showing all 9 members
- `player` — individual profile with badge progress
- `achievements` — all badges reference view
- `feed` — family activity feed with optional photos
- `map` — interactive SVG map of Kismet
- `fishing` — fishing species registry

**State:**
```javascript
state = {
  progress: {},   // badge progress per person
  fish: {},       // fish caught per person
  events: []      // feed events with optional photos (base64)
}
```

Firebase syncs this state in real time. Falls back to localStorage if Firebase is unavailable.

**People:** 9 family members with animal emoji avatars. Defined in `PEOPLE` and `PERSON_ICONS` arrays.

**Badges:** 16 categories, bronze/silver/gold tiers. Defined in the `BADGES` array. Some tiers auto-track from fishing species count or total legend badge count.

**Photos:** Stored as base64 strings directly in Firebase. Works fine for a small family group.

---

## Badge Content System

The badge system has four layers, from most specific to most universal:

| Layer | File | Description |
|-------|------|-------------|
| **Kismet-specific** | `src/index.html` (BADGES array) | Coded into the app. Kismet house only. |
| **Fire Island-wide** | `content/badges/fire_island.md` | Drafted. Works for all 17 Fire Island communities. |
| **City-specific** | `content/badges/nyc.md` | Drafted. NYC city-iconic badges + pocket neighborhood badges. |
| **Universal** | `content/badges/universal.md` | Drafted. Works for anyone, anywhere. |

**Supporting documents:**
- `content/badges/neighborhood_templates.md` — plug-and-play templates for any neighborhood, using [NEIGHBORHOOD] placeholder
- `content/badges/voice_guide.md` — rules and examples for writing good Kismet badges; use this when onboarding new curators or contributors

### Badge Voice (Summary)

Badges should be:
- **Clear** — you know unambiguously when you've done it
- **Specific** — specific enough to have teeth (add a number, a minimum, a constraint)
- **Interesting** — you actually want to do the thing
- **Simple** — no flowery language, no corporate wellness speak

Tiers: **Bronze = try it. Silver = go deeper. Gold = this is part of who you are.**

See `content/badges/voice_guide.md` for the full guide with annotated examples.

---

## The Bigger Picture

Kismet is a proof of concept for a **location-based personal fulfillment app** — a life well-lived tracker tied to place. The idea: every location has its own badge set. Universal badges travel with you everywhere. City-iconic badges unlock when you're in a city. Neighborhood pocket badges unlock when you're deep in a specific neighborhood.

The summer 2025 Kismet house is the internal beta. The product vision expands after summer.

**Do not build for the expanded product yet.** Focus on making the Kismet house app as good as possible for the family beta this summer. Every decision should serve that goal.

---

## People & Context

- **9 family members** using the app this summer
- **Family, not strangers** — the social dynamics are intimate; everyone knows everyone
- **Fire Island is seasonal** — the house is used May through September
- **The app is live and in use** — be careful with changes that affect existing data or localStorage keys

---
