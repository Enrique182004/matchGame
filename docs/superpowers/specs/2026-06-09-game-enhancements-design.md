# Game Enhancements — Design Spec

Date: 2026-06-09

## Overview

Enhance `game.html` with a countdown timer mode, a no-scroll 3-column layout, and a persistent score history. All changes are contained within `game.html` (single self-contained file, no build step).

## Page Flow (3 JS states, no navigation)

1. **Lobby** — timer picker + Start button (initial view on load)
2. **Game** — 3-column card layout, countdown running
3. **End screen** — modal overlay, round saved to history

History drawer can slide in from the right over any state.

---

## State 1: Lobby

Shown on initial page load and after "Play Again" from end screen.

- Gradient header (unchanged)
- Centered panel below header:
  - Title: "Choose your time limit"
  - Three large pill buttons: **1 min** / **3 min** / **5 min**
    - Sub-label per button: "Quick" / "Standard" / "Extended"
    - 3 min pre-selected by default
    - One selected at a time (toggle highlight)
  - **Start Game →** button (purple, large)
  - Small "View History" link below Start (opens history drawer)

---

## State 2: Game Layout

### Nav Bar (sticky)

- Home link (← Home)
- Countdown timer: large, centered — format `M:SS`
  - Normal: teal color
  - Under 30s: amber color
  - Under 15s: red color + pulse animation
- Score chip: "X matches"
- Streak chip: "🔥 X streak"
- History icon button (opens drawer)

### Main Area: 3-column grid

Fills remaining viewport height. Columns are independently scrollable.

| Column | Color  | Section           | Cards                             |
| ------ | ------ | ----------------- | --------------------------------- |
| 1      | Purple | Engineering Type  | 3 (IE, ME, SE) — no scroll needed |
| 2      | Teal   | Industry          | 14 — scrolls internally           |
| 3      | Coral  | Product / Problem | 16 — scrolls internally           |

Each column has:

- Step number badge + section title at top (sticky within column)
- Card grid below (wraps, internally scrollable)

### Sticky Bottom Bar (~100px)

Always visible. Contains:

- Three mini selection slots (eng / ind / prod) showing current picks or "—"
- Inline result area:
  - **Match**: green "✓ [Role Name]" with score/streak update
  - **No match**: red "✗ No match — try again" with shake
- **"Try Another"** button (resets selections only, keeps score/streak)

### Card Interaction

- Selecting all 3 → auto-evaluates immediately
- On match: confetti, score++, streak++, result shows in bottom bar
- On no-match: shake animation on bottom bar, streak resets
- Cards unclickable after timer reaches 0:00

---

## State 3: End-Game Overlay

Modal overlay (dark backdrop) triggered when countdown hits 0:00.

Contents:

- "Time's Up!" heading
- Round stats:
  - Matches found (this round)
  - Best streak (this round)
  - Timer duration chosen (e.g., "3 minute round")
- Two buttons:
  - **Play Again** → back to Lobby (resets score/streak/selections)
  - **View History** → opens history drawer (overlay stays or closes)
- Round auto-saved to localStorage on overlay show

---

## History

### Storage

- Key: `engineeringCareerHistory`
- Format: JSON array of objects: `{ date: string, duration: number (mins), matches: number, streak: number, timestamp: number }`
- Max 20 entries; oldest dropped when full
- Saved on end-game overlay appearance

### History Drawer

- Triggered by history icon in nav bar (any state) or "View History" button in end overlay
- Slides in from right side (CSS transform transition)
- Closes via X button or clicking backdrop
- Contents:
  - Header: "Game History"
  - List of entries (newest first): date, duration label, matches, streak
  - Empty state: "No games played yet"
  - "Clear History" button at bottom (with confirm)

---

## Data & JS State Variables

| Variable           | Type               | Purpose                          |
| ------------------ | ------------------ | -------------------------------- | ------------------------------ | ------------------------------ |
| `gameState`        | `'lobby'           | 'game'                           | 'end'`                         | Controls which view is visible |
| `selectedDuration` | `number` (1/3/5)   | Chosen timer duration in minutes |
| `timeRemaining`    | `number` (seconds) | Countdown value                  |
| `timerInterval`    | `number            | null`                            | setInterval handle             |
| `sel`              | `{eng, ind, prod}` | Current card selections          |
| `score`            | `number`           | Matches found this round         |
| `streak`           | `number`           | Current streak this round        |
| `bestStreak`       | `number`           | Best streak this round           |
| `roundStart`       | `number            | null`                            | Timestamp for per-match timing |
| `resultShown`      | `boolean`          | Gates card clicks after result   |

## localStorage Schema

```json
[
  {
    "date": "Jun 9, 2026",
    "duration": 3,
    "matches": 7,
    "streak": 4,
    "timestamp": 1749513600000
  }
]
```

---

## Files Changed

- `game.html` only — all changes are inline CSS + JS modifications
