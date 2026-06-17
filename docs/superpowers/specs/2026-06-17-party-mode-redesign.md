# Party Mode Redesign — Design Spec

Date: 2026-06-17

## Goals

1. Make every screen in `party.html` visually match `game.html` and `index.html`
2. Add game instructions to both lobby screens
3. Add a full-screen 3-2-1 countdown when the host starts the game
4. Improve mobile usability throughout

---

## Design System (from game.html / index.html)

- **Header gradient:** `linear-gradient(135deg, #26215c 0%, #534ab7 60%, #0f6e56 100%)`
- **Body background:** `#f5f4f0` (cream)
- **Nav bar:** `rgba(38, 33, 92, 0.97)` (dark purple, used for in-game top bar)
- **Text:** `#2c2c2a`
- **White cards:** `background: #fff; border: 1.5px solid #e0dfd8; border-radius: 12-14px`
- **Accent chips:** purple / teal / coral chip system already used in player game screen

---

## Screen-by-Screen Changes

### Home Screen (`#screen-home`)

- **Remove:** dark `#0f0e1a` body background
- **Add:** full-viewport layout with gradient header (top) + cream body (bottom)
- Header content: wrench emoji logo + `Engineering Career Explorer` h1 + `Party Mode` badge (matching `header-badge` style from game.html)
- Body: centered white card (`max-width: 420px`) containing name input, Create Game button, divider, code input, Join Game button, back link
- Inputs and buttons inherit the existing `.field` / `.btn` styles but on a light background (adjust borders to `#d3d1c7` instead of rgba white)

### Host Lobby (`#screen-host-lobby`)

- **Remove:** dark body, current `code-banner` purple gradient card
- **Add:** gradient header showing room code prominently
  - Header contains: "Room Code" label + large `code-big` text (white, same font size) + hint text
- Cream body (scrollable) contains in order:
  1. **Instructions panel** — white card, see Instructions section below
  2. **Players section** — section label + `chip-grid` of joined players (chips light-styled: white bg, gray border, dark text)
  3. **Duration picker** — section label + duration buttons (styled to match light theme)
  4. **Start Game button** — full-width, purple, disabled until ≥2 players

### Player Lobby (`#screen-player-lobby`)

- **Remove:** dark body, current centered wait-wrap with dark text
- **Add:** gradient header showing player's confirmed name + avatar emoji
  - Header: avatar + player name as h2 + "You're in!" subtext
- Cream body contains:
  1. **Instructions panel** — same component as host lobby
  2. **Fellow players section** — "In this room" label + `chip-grid` of all players
  3. **Waiting animation** — `Waiting for host to start...` with animated dots, centered, muted text

### Host Game Screen (`#screen-game-host`)

- **Remove:** dark `bg-dark` background
- **Add:** dark purple nav bar at top (matching `.game-topbar` / `.nav-bar` style from game.html)
  - Nav: left = "Party Mode 👑 Host" label; center = timer (`host-timer`, large, teal/amber/red states); right = total matches found chip
- Cream body below nav:
  - "Live Leaderboard" section label
  - `lb-row` cards on cream/white background (adjust from `rgba(255,255,255,0.07)` dark to white card with `#e0dfd8` border)
  - Rank medal, player name, score in teal

### Player Game Screen (`#screen-game-player`)

- Already uses light theme — minimal changes needed
- **Improve:** top bar padding, chip sizes for mobile touch targets
- **Add:** slight shadow under tabs for depth

### Final Screen (`#screen-final`)

- **Remove:** dark `bg-dark` background
- **Add:** gradient header with trophy + "Final Results" title
- Cream body: podium (keep metal colors), full leaderboard as white cards
- "Play Again" button styled as btn-purple

---

## Instructions Panel Component

Reusable white card inserted into both lobby screens.

```
┌─────────────────────────────────────────┐
│  🎮  How to Play                        │
│                                         │
│  Select one card from each of 3 tabs:   │
│  • Engineering Specialty  (tab 1)       │
│  • Industry               (tab 2)       │
│  • Work Focus             (tab 3)       │
│                                         │
│  If your 3-card combo matches a real    │
│  engineering career → you score 1 pt!  │
│                                         │
│  Find as many combos as possible        │
│  before time runs out. 🏆              │
└─────────────────────────────────────────┘
```

Styled as: white card, border `#e0dfd8`, border-radius 14px, padding 1.25rem, `max-width: 480px`, width 100%.

---

## Countdown Overlay

### Trigger

`listenRoom()` detects `status === "playing"` for the first time. Instead of immediately calling `show("screen-game-host/player")`, it calls `startCountdown(callback)` which shows the overlay for ~3.5s then calls the callback.

### Visual

- `position: fixed; inset: 0; z-index: 9000`
- Background: `rgba(38, 33, 92, 0.95)` (dark purple, semi-transparent)
- Centered flex column: large number + label below
- Number: `font-size: clamp(120px, 25vw, 180px); font-weight: 900; color: #fff`
- Sequence: **3** (900ms) → **2** (900ms) → **1** (900ms) → **GO!** (700ms) → hide
- Each step: scale-in animation (`transform: scale(0.4) → scale(1)`, 200ms ease-out) then scale-out (`scale(1) → scale(1.3)`, 200ms ease-in) before next number
- Color shifts: 3 = white, 2 = `#5dcaa5` (teal), 1 = `#f09595` (red), GO! = `#ffd700` (gold)

### Implementation

```js
function startCountdown(cb) {
  // show overlay, cycle 3→2→1→GO!, then call cb() and hide overlay
}
```

Called in `listenRoom()` where `gameStartTime === 0` before showing game screen.

---

## Mobile Improvements

| Element            | Current       | Target                       |
| ------------------ | ------------- | ---------------------------- |
| Card min-height    | 66px          | 76px                         |
| Card icon size     | 18px          | 20px                         |
| Card label size    | 10px          | 11px                         |
| Tab button height  | ~38px         | min 44px                     |
| All buttons/cards  | no touch hint | `touch-action: manipulation` |
| Bottom slot labels | 10px          | 11px, better overflow        |
| Home inputs        | 16px          | 16px ✓ (already ok)          |

---

## CSS Architecture

- Remove `:root { --bg-dark }` and all dark-background rules for home/lobby/final screens
- Add gradient header styles matching `game.html`'s `header { }` block
- Add `.instructions-card` component class
- Add `#countdown-overlay` styles
- Adjust `.chip` to light-theme version: `background: #fff; border: 1.5px solid #d3d1c7; color: #2c2c2a`
- Adjust `.lb-row` to light-theme: `background: #fff; border: 1.5px solid #e0dfd8`
- Adjust duration buttons `.dur-btn` to light-theme

---

## Files Changed

- `party.html` — sole file, all CSS and JS inline

## Files Unchanged

- `game.html`, `index.html`, `careers.html` — no changes
