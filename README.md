# LaneIQ — WellPutt Practice Tracker

A local-first Progressive Web App that turns reps on your **WellPutt 10ft mat** into a game — part of the **GolfIQ suite**. One idea drives every screen: **make practice count, one number at a time.**

**Live app → [ifunner.github.io/laneiq](https://ifunner.github.io/laneiq/)** *(pending first deploy)*

---

## What it does

LaneIQ maps drills to the real marks and **numbered zones** on your WellPutt 10ft mat and scores every rep, so aimless putting becomes a session you can beat next time. Log makes, Wellputt-Zone speed, and start-line reps; play the WellPutt zone games and courses; and watch your make rates, speed bias, and streaks move over time.

No accounts, no servers. Everything stays on your device and works offline next to the mat.

## Features

- **Core drills, mapped to the mat** — **Make Test** (hole out from 3 / 4.5 / 6 / 7.5 / 10 ft), **Good Zone** (roll it past into the Wellputt Zone — and read which third it settles in: **downhill · level · uphill**), and **Start Line** (send it through the gate on your aim line).
- **Zone games** — built on the mat's numbered speed zones: **Zone Ladder** (climb 1 → 12; a miss resets you) and **Zone Call** (announce a zone, stop it there — short costs points, WellPutt-style).
- **The 21** — a daily routine: 7 start-line · 7 makes from 6 ft · 7 Wellputt-Zone. One pass, one number.
- **18-hole courses** — the WellPutt progression: **Orange** (foundations), **Blue** (the Wellputt Zone), **Black** (pressure), mixing hole-outs, zone-finishes and stop-in-zone targets, with to-par scoring and gold/silver/bronze medals.
- **Progression** — XP, levels, ranks, and unlockable achievements with progress bars.
- **Stats** — make rate by distance, speed control across the Wellputt Zone (short-rate + downhill / level / uphill), start-line %, zone-game bests, and a make-rate trend.
- **History & streaks** — sessions grouped by day; day streak tracking; replay a course from any row.
- **Mat guide** — a labelled diagram of both play directions, the three-part Wellputt Zone, and the numbered speed zones.

## GolfIQ suite

LaneIQ is the **putting-rep side** of GolfIQ — turn WellPutt mat reps into a game and track them over time.

- **[StrokesIQ](https://ifunner.github.io/strokesiq/)** — strokes gained round tracking; names your biggest leak and scoring potential.
- **[PracticeIQ](https://ifunner.github.io/practiceiq/)** — practice planner and session logger; builds routines (including a WellPutt block) around your leak.
- **[GreenIQ](https://ifunner.github.io/greeniq/)** — green reading and putting trainer; slope, aim, pace, and feel on the practice green.

## Install on your phone

1. Open **[ifunner.github.io/laneiq](https://ifunner.github.io/laneiq/)** in Safari (iOS) or Chrome (Android).
2. Share → **Add to Home Screen**.
3. Launch from the home-screen icon — it runs full-screen and offline after first load.

PWA install and offline only work over **HTTPS** — opening `index.html` as a local file will not work.

## Note

LaneIQ stores everything in your browser's local storage (with an in-memory fallback). Clearing site data or an iOS storage eviction will reset your history — treat a long streak like anything else on your phone.

---

## For developers

Static single-file PWA — vanilla JavaScript, no framework, no build step, no backend, no tracking.

| File / folder | Role |
|---|---|
| `index.html` | App shell, UI, and engine (storage, drill runner, course + XP/achievement logic) |
| `manifest.json` | PWA manifest |
| `sw.js` | Offline app-shell cache |
| `icon.svg` | Master LaneIQ brand mark |
| `icon-192.png` · `icon-512.png` · `icon-maskable-512.png` · `apple-touch-icon.png` · `favicon.png` | PWA / home-screen icons |

### Deploy (GitHub Pages)

1. Push to `main` with the repo root as the Pages source (keep the icon files at the root — `manifest.json` references them relatively).
2. Open the published `https://…` URL.

**Netlify:** drag the project folder onto [app.netlify.com/drop](https://app.netlify.com/drop).

### Design system

Aligned to the shared GolfIQ language: felt background, Space Grotesk + system mono, **flag-yellow** reserved for primary CTAs / the active tab / the cup, and **path-green** for success and ball paths. Tokens are inlined in `index.html` rather than vendoring `golfiq.css`.

Design tokens and cross-suite UI rules: [`golfiq-design`](https://github.com/ifunner/golfiq-design) · [`DESIGN_SYSTEM.md`](https://github.com/ifunner/golfiq-design/blob/main/DESIGN_SYSTEM.md).
