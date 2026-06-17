# Party Mode Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Retheme all screens in `party.html` to match `game.html`/`index.html` (gradient header + cream body), add instructions panels to both lobbies, add a full-screen 3-2-1 countdown when the game starts, and improve mobile touch targets.

**Architecture:** All changes are inside `party.html` (single file, inline CSS + HTML + JS). CSS tasks come first so HTML restructuring can immediately verify visually. JS countdown is added last as it layers on top of the existing `listenRoom()` logic.

**Tech Stack:** Vanilla HTML/CSS/JS, Firebase Realtime Database (existing), Tabler Icons CDN (existing)

**Spec:** `docs/superpowers/specs/2026-06-17-party-mode-redesign.md`

---

## File Map

| File                                       | Change                                                                                                               |
| ------------------------------------------ | -------------------------------------------------------------------------------------------------------------------- |
| `party.html` (CSS block, lines ~13–824)    | Remove dark-theme rules; add gradient header, light lobby, instructions card, countdown overlay, mobile improvements |
| `party.html` (HTML block, lines ~826–1014) | Restructure home, host-lobby, player-lobby, host-game, final screens                                                 |
| `party.html` (JS block, lines ~1016–2026)  | Add `startCountdown()`, update `listenRoom()` to use it, update `renderLobbyPlayers()` for dual p-count              |

---

## Task 1: CSS — Base Theme (remove dark, add gradient header + light body)

**Files:**

- Modify: `party.html` — `:root`, `body`, `.screen`, `.home-wrap`, `.back-lnk`, `.toast`, `.btn-ghost`

- [ ] **Step 1: Replace `:root` and base body rules**

Find and replace the entire `:root` block and `html, body` + `body` rules in the `<style>` tag. The new version removes `--bg-dark`, keeps all accent variables, and makes the body light:

```css
:root {
  --purple: #534ab7;
  --purple-dk: #3c3489;
  --purple-50: #eeedfe;
  --teal: #0f6e56;
  --teal-50: #e1f5ee;
  --teal-200: #5dcaa5;
  --coral: #993c1d;
  --coral-50: #faece7;
  --coral-200: #f0997b;
  --gray-50: #f1efe8;
  --gray-100: #d3d1c7;
  --gray-400: #888780;
  --gray-600: #5f5e5a;
  --green-50: #ecfdf5;
  --green-600: #059669;
  --red-50: #fcebeb;
  --red-800: #791f1f;
}
html,
body {
  height: 100%;
  font-family: "Segoe UI", system-ui, sans-serif;
}
body {
  background: #f5f4f0;
  color: #2c2c2a;
}
.screen {
  display: none;
  min-height: 100dvh;
  flex-direction: column;
  align-items: stretch;
}
.screen.active {
  display: flex;
}
/* Shared gradient header (home, lobbies, final) */
.party-header {
  background: linear-gradient(135deg, #26215c 0%, #534ab7 60%, #0f6e56 100%);
  padding: 2.5rem 1.25rem 2rem;
  text-align: center;
  color: #fff;
  flex-shrink: 0;
  position: relative;
  overflow: hidden;
}
.party-header::before {
  content: "";
  position: absolute;
  top: -50px;
  right: -50px;
  width: 200px;
  height: 200px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.05);
}
.header-badge {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  background: rgba(255, 255, 255, 0.15);
  border: 1px solid rgba(255, 255, 255, 0.25);
  border-radius: 20px;
  padding: 3px 12px;
  font-size: 11px;
  font-weight: 500;
  color: rgba(255, 255, 255, 0.9);
  margin-bottom: 10px;
  letter-spacing: 0.04em;
  position: relative;
  z-index: 1;
}
.party-header .logo {
  font-size: 40px;
  margin-bottom: 8px;
  position: relative;
  z-index: 1;
}
.party-header h1 {
  font-size: clamp(16px, 4vw, 22px);
  font-weight: 700;
  margin-bottom: 4px;
  position: relative;
  z-index: 1;
}
.party-header h2 {
  font-size: 13px;
  color: rgba(255, 255, 255, 0.65);
  position: relative;
  z-index: 1;
}
/* Scrollable body region below header */
.screen-body {
  flex: 1;
  overflow-y: auto;
  padding: 1.5rem 1.25rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.25rem;
}
```

- [ ] **Step 2: Replace home-wrap, field, btn, divider, toast, back-lnk**

Replace the `/* ── HOME ──*/` block and related rules with light-themed versions:

```css
/* ── HOME CARD ── */
.home-card {
  background: #fff;
  border: 1.5px solid #e0dfd8;
  border-radius: 16px;
  padding: 1.75rem 1.5rem;
  width: 100%;
  max-width: 420px;
  display: flex;
  flex-direction: column;
  gap: 0;
}
.field {
  width: 100%;
  padding: 13px 16px;
  background: #faf9f7;
  border: 1.5px solid #d3d1c7;
  border-radius: 10px;
  color: #2c2c2a;
  font-size: 16px;
  font-family: inherit;
  margin-bottom: 10px;
  text-align: center;
}
.field::placeholder {
  color: #888780;
}
.field:focus {
  outline: none;
  border-color: var(--purple);
}
#code-input {
  letter-spacing: 0.2em;
  font-weight: 700;
  font-size: 22px;
  text-transform: uppercase;
}
.btn {
  width: 100%;
  padding: 13px 20px;
  border: none;
  border-radius: 12px;
  font-size: 15px;
  font-weight: 700;
  cursor: pointer;
  font-family: inherit;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  margin-bottom: 10px;
  transition: filter 0.12s;
  touch-action: manipulation;
}
.btn:hover:not(:disabled) {
  filter: brightness(1.08);
}
.btn:disabled {
  opacity: 0.4;
  cursor: not-allowed;
}
.btn-purple {
  background: var(--purple);
  color: #fff;
}
.btn-ghost {
  background: #f1efe8;
  color: #444441;
  border: 1.5px solid #d3d1c7;
}
.btn-lg {
  padding: 16px 20px;
  font-size: 18px;
}
.divider {
  display: flex;
  align-items: center;
  gap: 10px;
  color: #888780;
  font-size: 12px;
  margin: 6px 0 14px;
}
.divider::before,
.divider::after {
  content: "";
  flex: 1;
  height: 1px;
  background: #d3d1c7;
}
.toast {
  background: #a32d2d;
  color: #fff;
  padding: 10px 16px;
  border-radius: 10px;
  font-size: 13px;
  font-weight: 600;
  margin-top: 10px;
  display: none;
}
.back-lnk {
  color: #888780;
  font-size: 12px;
  text-decoration: none;
  margin-top: 8px;
  display: block;
  text-align: center;
}
.back-lnk:hover {
  color: #2c2c2a;
}
```

- [ ] **Step 3: Open `party.html` in a browser and verify home screen visually**

Open the file directly (`open party.html` or drag to browser). You should see:

- Gradient header at the top with the 🛠 logo and "Engineering Career Explorer" title
- Cream body with a white card containing the name input and buttons
- No dark backgrounds

- [ ] **Step 4: Commit**

```bash
git add party.html
git commit -m "style: retheme party base — light body, gradient header, home card"
```

---

## Task 2: CSS — Lobby Styles (chips, duration buttons, section labels, code display)

**Files:**

- Modify: `party.html` — lobby CSS block (`.code-banner`, `.chip`, `.dur-btn`, `.sec-lbl`)

- [ ] **Step 1: Replace lobby CSS rules**

Find the `/* ── LOBBY ──*/` block and replace entirely:

```css
/* ── LOBBY ── */
#screen-host-lobby {
  align-items: stretch;
}
#screen-player-lobby {
  align-items: stretch;
}

.code-display {
  text-align: center;
  padding: 0.5rem 0;
  position: relative;
  z-index: 1;
}
.code-label {
  font-size: 11px;
  letter-spacing: 0.12em;
  color: rgba(255, 255, 255, 0.65);
  text-transform: uppercase;
  margin-bottom: 4px;
}
.code-big {
  font-size: clamp(42px, 12vw, 58px);
  font-weight: 900;
  letter-spacing: 0.18em;
  line-height: 1;
  color: #fff;
}
.code-hint {
  font-size: 12px;
  color: rgba(255, 255, 255, 0.5);
  margin-top: 6px;
}
.sec-lbl {
  font-size: 12px;
  font-weight: 600;
  color: #5f5e5a;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  align-self: flex-start;
  width: 100%;
  max-width: 560px;
}
.chip-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  justify-content: flex-start;
  width: 100%;
  max-width: 560px;
}
.chip {
  display: flex;
  align-items: center;
  gap: 7px;
  padding: 7px 14px;
  background: #fff;
  border: 1.5px solid #d3d1c7;
  border-radius: 20px;
  font-size: 14px;
  font-weight: 500;
  color: #2c2c2a;
}
.dur-row {
  display: flex;
  gap: 8px;
  justify-content: flex-start;
  flex-wrap: wrap;
  width: 100%;
  max-width: 560px;
}
.dur-btn {
  padding: 10px 20px;
  border: 1.5px solid #d3d1c7;
  border-radius: 10px;
  background: #fff;
  color: #5f5e5a;
  font-size: 14px;
  font-weight: 700;
  cursor: pointer;
  font-family: inherit;
  transition: all 0.12s;
  touch-action: manipulation;
}
.dur-btn.on {
  border-color: var(--purple);
  background: var(--purple);
  color: #fff;
}
#start-btn {
  max-width: 560px;
}

/* ── PLAYER LOBBY ── */
.wait-icon {
  font-size: 52px;
  margin-bottom: 6px;
  position: relative;
  z-index: 1;
}
.wait-name {
  font-size: 24px;
  font-weight: 800;
  color: #fff;
  position: relative;
  z-index: 1;
}
.wait-msg {
  color: #5f5e5a;
  font-size: 15px;
  text-align: center;
  padding: 0.5rem 0;
}
.dot {
  display: inline-block;
  animation: blink 1.4s infinite;
}
.dot:nth-child(2) {
  animation-delay: 0.2s;
}
.dot:nth-child(3) {
  animation-delay: 0.4s;
}
@keyframes blink {
  0%,
  80%,
  100% {
    opacity: 0;
  }
  40% {
    opacity: 1;
  }
}
```

- [ ] **Step 2: Verify in browser**

Navigate to the host lobby screen by temporarily adding `class="screen active"` to `#screen-host-lobby`. You should see:

- No dark backgrounds in the body region
- White chips with gray borders
- Duration buttons with gray borders (purple when `.on`)

Revert the temporary class after verifying.

- [ ] **Step 3: Commit**

```bash
git add party.html
git commit -m "style: light-theme lobby chips, duration buttons, code display"
```

---

## Task 3: CSS — Instructions Card + Countdown Overlay

**Files:**

- Modify: `party.html` — add new CSS rules after existing lobby block

- [ ] **Step 1: Add instructions card CSS**

Add after the `/* ── PLAYER LOBBY ──*/` block:

```css
/* ── INSTRUCTIONS CARD ── */
.instructions-card {
  background: #fff;
  border: 1.5px solid #e0dfd8;
  border-radius: 14px;
  padding: 1.25rem 1.5rem;
  width: 100%;
  max-width: 560px;
}
.instructions-card h3 {
  font-size: 14px;
  font-weight: 700;
  color: var(--purple);
  margin-bottom: 10px;
  display: flex;
  align-items: center;
  gap: 7px;
}
.instructions-card ol {
  padding-left: 1.25rem;
  display: flex;
  flex-direction: column;
  gap: 7px;
}
.instructions-card li {
  font-size: 13px;
  color: #444441;
  line-height: 1.5;
}
.instructions-card li strong {
  color: #2c2c2a;
}
.instructions-tip {
  margin-top: 10px;
  font-size: 12px;
  color: #888780;
  background: #f5f4f0;
  border-radius: 8px;
  padding: 8px 12px;
}

/* ── COUNTDOWN OVERLAY ── */
#countdown-overlay {
  position: fixed;
  inset: 0;
  z-index: 9000;
  background: rgba(38, 33, 92, 0.95);
  display: none;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 12px;
}
.countdown-num {
  font-size: clamp(100px, 25vw, 180px);
  font-weight: 900;
  line-height: 1;
  will-change: transform, opacity;
}
.countdown-sub {
  font-size: 18px;
  color: rgba(255, 255, 255, 0.6);
  font-weight: 500;
}
@keyframes countPop {
  0% {
    transform: scale(0.3);
    opacity: 0;
  }
  25% {
    transform: scale(1.1);
    opacity: 1;
  }
  65% {
    transform: scale(1);
    opacity: 1;
  }
  100% {
    transform: scale(1.4);
    opacity: 0;
  }
}
```

- [ ] **Step 2: Commit**

```bash
git add party.html
git commit -m "style: add instructions card and countdown overlay CSS"
```

---

## Task 4: CSS — Host Game Light Theme + Final Light Theme + Mobile Polish

**Files:**

- Modify: `party.html` — `/* ── HOST GAME SCREEN ──*/`, `/* ── FINAL ──*/`, `@media` block

- [ ] **Step 1: Replace host game screen CSS**

Find `/* ── HOST GAME SCREEN ──*/` and replace:

```css
/* ── HOST GAME SCREEN ── */
#screen-game-host {
  align-items: stretch;
}
.host-topbar {
  background: rgba(38, 33, 92, 0.97);
  padding: 10px 1.25rem;
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-shrink: 0;
  color: #fff;
}
.host-timer {
  font-size: 42px;
  font-weight: 900;
  color: var(--teal-200);
  letter-spacing: -0.04em;
  line-height: 1;
}
.host-timer.amber {
  color: #f5c67a;
}
.host-timer.red {
  color: #f09595;
  animation: pulse 0.8s ease infinite;
}
@keyframes pulse {
  0%,
  100% {
    opacity: 1;
  }
  50% {
    opacity: 0.4;
  }
}
.host-meta-lbl {
  font-size: 11px;
  color: rgba(255, 255, 255, 0.5);
}
.host-meta-val {
  font-size: 20px;
  font-weight: 800;
  color: #fff;
}
.host-lb-title {
  font-size: 12px;
  font-weight: 600;
  color: #5f5e5a;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  margin-bottom: 10px;
}
.lb-row {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px 16px;
  background: #fff;
  border: 1.5px solid #e0dfd8;
  border-radius: 12px;
  margin-bottom: 8px;
  transition: all 0.3s;
}
.lb-row.me {
  background: var(--purple-50);
  border-color: var(--purple);
}
.lb-rank {
  font-size: 20px;
  font-weight: 900;
  width: 32px;
  text-align: center;
  color: #2c2c2a;
}
.lb-name {
  flex: 1;
  font-size: 16px;
  font-weight: 600;
  color: #2c2c2a;
}
.lb-score {
  font-size: 20px;
  font-weight: 800;
  color: var(--teal);
}
```

- [ ] **Step 2: Replace final screen CSS**

Find `/* ── FINAL ──*/` and replace:

```css
/* ── FINAL ── */
#screen-final {
  align-items: stretch;
}
.final-ttl {
  font-size: clamp(22px, 6vw, 32px);
  font-weight: 900;
  color: #fff;
  position: relative;
  z-index: 1;
}
.podium {
  display: flex;
  align-items: flex-end;
  justify-content: center;
  gap: 10px;
  width: 100%;
  max-width: 400px;
}
.pod-col {
  text-align: center;
}
.pod-name {
  font-size: 12px;
  font-weight: 700;
  margin-bottom: 5px;
  max-width: 80px;
  word-break: break-word;
  color: #2c2c2a;
}
.pod-blk {
  border-radius: 12px 12px 0 0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 26px;
  width: 80px;
}
.pod-1 {
  background: #ffd700;
  height: 100px;
}
.pod-2 {
  background: #c0c0c0;
  height: 70px;
}
.pod-3 {
  background: #cd7f32;
  height: 50px;
}
.pod-score {
  font-size: 11px;
  color: #888780;
  margin-top: 4px;
}
.full-lb {
  width: 100%;
  max-width: 480px;
}
```

- [ ] **Step 3: Replace / extend media query with mobile polish**

Find the existing `@media (max-width: 480px)` block and replace:

```css
@media (max-width: 520px) {
  .party-header {
    padding: 1.75rem 1rem 1.5rem;
  }
  .code-big {
    font-size: 36px;
  }
  .card {
    min-height: 76px;
  }
  .card i {
    font-size: 20px;
  }
  .card span {
    font-size: 11px;
  }
  .col-tab {
    min-height: 44px;
    font-size: 11px;
    padding: 10px 4px;
  }
  .slot-lbl {
    font-size: 11px;
  }
  .home-card {
    padding: 1.25rem 1rem;
  }
  .instructions-card {
    padding: 1rem 1.1rem;
  }
}
```

Also add `touch-action: manipulation` to the `.card` rule (find `.card {` and add the property):

```css
.card {
  /* existing properties ... */
  touch-action: manipulation;
}
```

- [ ] **Step 4: Commit**

```bash
git add party.html
git commit -m "style: light-theme host game, final screen, mobile polish"
```

---

## Task 5: HTML — Home Screen Restructure

**Files:**

- Modify: `party.html` — `<!-- HOME -->` block

- [ ] **Step 1: Replace the home screen HTML**

Find `<!-- HOME -->` and replace the entire `<div id="screen-home" ...>` element:

```html
<!-- HOME -->
<div id="screen-home" class="screen active">
  <header class="party-header">
    <div class="header-badge"><i class="ti ti-users"></i> Party Mode</div>
    <div class="logo">🛠</div>
    <h1>Engineering Career Explorer</h1>
    <h2>Compete with friends to find career combos</h2>
  </header>
  <div class="screen-body">
    <div class="home-card">
      <input
        class="field"
        id="player-name"
        placeholder="Your name"
        maxlength="20"
        autocomplete="off"
      />
      <button class="btn btn-purple btn-lg" onclick="createGame()">
        <i class="ti ti-plus"></i> Create Game
      </button>
      <div class="divider">or join with a code</div>
      <input
        class="field"
        id="code-input"
        placeholder="ROOM CODE"
        maxlength="6"
        autocomplete="off"
      />
      <button class="btn btn-ghost" onclick="joinGame()">
        <i class="ti ti-door-enter"></i> Join Game
      </button>
      <div class="toast" id="home-err"></div>
      <a class="back-lnk" href="index.html">← Back to solo mode</a>
    </div>
  </div>
</div>
```

- [ ] **Step 2: Verify in browser**

Open `party.html`. The home screen should show:

- Purple-to-teal gradient header with wrench emoji, title, subtitle
- White card below on cream background with inputs and buttons

- [ ] **Step 3: Commit**

```bash
git add party.html
git commit -m "feat: restructure home screen to gradient header + light body"
```

---

## Task 6: HTML — Host Lobby Restructure

**Files:**

- Modify: `party.html` — `<!-- HOST LOBBY -->` block

- [ ] **Step 1: Replace host lobby HTML**

Find `<!-- HOST LOBBY -->` and replace the entire `<div id="screen-host-lobby" ...>` element:

```html
<!-- HOST LOBBY -->
<div id="screen-host-lobby" class="screen">
  <header class="party-header">
    <div class="header-badge"><i class="ti ti-crown"></i> Host Lobby</div>
    <div class="code-display">
      <div class="code-label">Room Code</div>
      <div class="code-big" id="disp-code">XXXXXX</div>
      <div class="code-hint">
        Players open this page and enter the code above
      </div>
    </div>
  </header>
  <div class="screen-body">
    <!-- Instructions -->
    <div class="instructions-card">
      <h3><i class="ti ti-help-circle"></i> How to Play</h3>
      <ol>
        <li>
          Select one card from <strong>Engineering Specialty</strong> (Tab 1)
        </li>
        <li>Select one card from <strong>Industry</strong> (Tab 2)</li>
        <li>Select one card from <strong>Work Focus</strong> (Tab 3)</li>
        <li>
          If all three match a real engineering career →
          <strong>+1 point!</strong>
        </li>
      </ol>
      <div class="instructions-tip">
        🏆 Find as many combinations as you can before time runs out. Fastest
        fingers win!
      </div>
    </div>
    <!-- Players -->
    <div class="sec-lbl">Players joined (<span id="p-count">0</span>)</div>
    <div class="chip-grid" id="p-grid"></div>
    <!-- Duration -->
    <div class="sec-lbl">Game duration</div>
    <div class="dur-row">
      <button class="dur-btn" onclick="setDur(60, this)">1 min</button>
      <button class="dur-btn on" onclick="setDur(180, this)">3 min</button>
      <button class="dur-btn" onclick="setDur(300, this)">5 min</button>
    </div>
    <button
      class="btn btn-purple btn-lg"
      id="start-btn"
      onclick="hostStart()"
      disabled
    >
      <i class="ti ti-player-play"></i> Start Game
    </button>
  </div>
</div>
```

- [ ] **Step 2: Verify in browser**

Temporarily change `id="screen-host-lobby"` to also include `class="screen active"` (and remove from home). You should see:

- Gradient header with room code XXXXXX displayed large
- Cream body with instructions card, empty player chips area, duration buttons, disabled Start button

Revert the class change.

- [ ] **Step 3: Commit**

```bash
git add party.html
git commit -m "feat: restructure host lobby — instructions panel + light theme"
```

---

## Task 7: HTML — Player Lobby Restructure

**Files:**

- Modify: `party.html` — `<!-- PLAYER LOBBY -->` block

- [ ] **Step 1: Replace player lobby HTML**

Find `<!-- PLAYER LOBBY -->` and replace the entire `<div id="screen-player-lobby" ...>` element:

```html
<!-- PLAYER LOBBY -->
<div id="screen-player-lobby" class="screen">
  <header class="party-header">
    <div class="header-badge"><i class="ti ti-check"></i> You're In!</div>
    <div class="wait-icon">🎮</div>
    <div class="wait-name" id="wait-name">Player</div>
    <h2>Get ready to play</h2>
  </header>
  <div class="screen-body">
    <!-- Instructions -->
    <div class="instructions-card">
      <h3><i class="ti ti-help-circle"></i> How to Play</h3>
      <ol>
        <li>
          Select one card from <strong>Engineering Specialty</strong> (Tab 1)
        </li>
        <li>Select one card from <strong>Industry</strong> (Tab 2)</li>
        <li>Select one card from <strong>Work Focus</strong> (Tab 3)</li>
        <li>
          If all three match a real engineering career →
          <strong>+1 point!</strong>
        </li>
      </ol>
      <div class="instructions-tip">
        🏆 Find as many combinations as you can before time runs out. Fastest
        fingers win!
      </div>
    </div>
    <!-- Fellow players -->
    <div class="sec-lbl">Players in this room</div>
    <div class="chip-grid" id="fellow-grid"></div>
    <!-- Waiting -->
    <div class="wait-msg">
      Waiting for host to start<span class="dot">.</span
      ><span class="dot">.</span><span class="dot">.</span>
    </div>
  </div>
</div>
```

- [ ] **Step 2: Commit**

```bash
git add party.html
git commit -m "feat: restructure player lobby — instructions panel + light theme"
```

---

## Task 8: HTML — Host Game Screen + Final Screen Restructure

**Files:**

- Modify: `party.html` — `<!-- HOST GAME -->` and `<!-- FINAL -->` blocks

- [ ] **Step 1: Replace host game screen HTML**

Find `<!-- HOST GAME (projected leaderboard) -->` and replace the entire element:

```html
<!-- HOST GAME (projected leaderboard) -->
<div id="screen-game-host" class="screen">
  <!-- Dark nav bar -->
  <div class="host-topbar">
    <div>
      <div class="host-meta-lbl">Time left</div>
      <div class="host-timer" id="host-timer">3:00</div>
    </div>
    <div style="text-align:center">
      <div class="host-meta-lbl">Party Mode</div>
      <div style="font-size:15px;font-weight:700;color:#fff">👑 Host View</div>
    </div>
    <div style="text-align:right">
      <div class="host-meta-lbl">Matches found</div>
      <div class="host-meta-val" id="host-total-matches">0</div>
    </div>
  </div>
  <!-- Light body -->
  <div class="screen-body">
    <div style="width:100%;max-width:640px">
      <div class="host-lb-title">Live Leaderboard</div>
      <div id="host-lb-rows"></div>
    </div>
  </div>
</div>
```

- [ ] **Step 2: Replace final screen HTML**

Find `<!-- FINAL -->` and replace the entire element:

```html
<!-- FINAL -->
<div id="screen-final" class="screen">
  <header class="party-header">
    <div class="header-badge"><i class="ti ti-trophy"></i> Final Results</div>
    <div class="final-ttl">🏆 Game Over!</div>
  </header>
  <div class="screen-body">
    <div class="podium" id="podium"></div>
    <div class="full-lb" id="full-lb"></div>
    <button class="btn btn-purple" onclick="goHome()" style="max-width:280px">
      <i class="ti ti-refresh"></i> Play Again
    </button>
  </div>
</div>
```

- [ ] **Step 3: Commit**

```bash
git add party.html
git commit -m "feat: restructure host game + final screen to light theme"
```

---

## Task 9: HTML + JS — Countdown Overlay + startCountdown()

**Files:**

- Modify: `party.html` — add overlay element to `<body>`, add `startCountdown()` JS function, update `listenRoom()` JS function

- [ ] **Step 1: Add countdown overlay HTML**

Add this element as the last child of `<body>`, just before `<script>`:

```html
<!-- COUNTDOWN OVERLAY -->
<div id="countdown-overlay">
  <div id="countdown-num" class="countdown-num">3</div>
  <div class="countdown-sub" id="countdown-sub">Get ready!</div>
</div>
```

- [ ] **Step 2: Add startCountdown() function to JS**

Add this function in the JS block, after the `endPlayerGame()` function (around line 1794):

```js
// ── COUNTDOWN ─────────────────────────────────────────────────────────────
function startCountdown(cb) {
  const overlay = document.getElementById("countdown-overlay");
  const numEl = document.getElementById("countdown-num");
  const subEl = document.getElementById("countdown-sub");
  const colors = { 3: "#ffffff", 2: "#5dcaa5", 1: "#f09595" };

  overlay.style.display = "flex";
  let n = 3;

  function step() {
    if (n > 0) {
      numEl.textContent = n;
      numEl.style.color = colors[n];
      subEl.textContent = "Get ready!";
      numEl.style.animation = "none";
      void numEl.offsetHeight; // force reflow to restart animation
      numEl.style.animation = "countPop 0.9s ease forwards";
      n--;
      setTimeout(step, 900);
    } else {
      numEl.textContent = "GO!";
      numEl.style.color = "#ffd700";
      subEl.textContent = "Find the combos!";
      numEl.style.animation = "none";
      void numEl.offsetHeight;
      numEl.style.animation = "countPop 0.7s ease forwards";
      setTimeout(() => {
        overlay.style.display = "none";
        cb();
      }, 700);
    }
  }

  step();
}
```

- [ ] **Step 3: Update listenRoom() to call startCountdown()**

Find the `if (gameStartTime === 0)` block inside `listenRoom()` and replace it:

```js
if (gameStartTime === 0) {
  gameStartTime = room.gameStart;
  gameDuration = room.duration;
  if (myRole === "host") {
    startCountdown(() => {
      show("screen-game-host");
      startHostTimer(room.gameStart, room.duration);
    });
  } else {
    startCountdown(() => {
      show("screen-game-player");
      startPlayerTimer(room.gameStart, room.duration);
    });
  }
}
```

- [ ] **Step 4: Verify countdown manually in browser**

Add this temporary call in the browser console to test the countdown without a Firebase connection:

```js
startCountdown(() => console.log("countdown done"));
```

You should see: 3 (white, scales in/out) → 2 (teal) → 1 (red) → GO! (gold) → console log fires.

- [ ] **Step 5: Commit**

```bash
git add party.html
git commit -m "feat: add 3-2-1-GO countdown overlay before game start"
```

---

## Task 10: Full End-to-End Verification

- [ ] **Step 1: Open party.html and test home screen**
  - Gradient header visible, white card on cream body ✓
  - Name input, Create/Join buttons styled correctly ✓
  - "Back to solo mode" link visible ✓

- [ ] **Step 2: Test host lobby (create a game)**
  - Enter a name, click "Create Game"
  - Room code appears large in gradient header ✓
  - Instructions card visible in body ✓
  - Duration buttons light-styled ✓
  - Start button disabled until 2 players ✓

- [ ] **Step 3: Open a second tab/incognito, join the room**
  - Player lobby shows gradient header with player name ✓
  - Instructions card visible ✓
  - Both player chips appear in both lobbies ✓

- [ ] **Step 4: Host clicks Start Game**
  - Both tabs show countdown overlay: 3 → 2 → 1 → GO! ✓
  - Host transitions to host game screen (dark purple nav + cream leaderboard) ✓
  - Player transitions to player game screen (already light-themed) ✓

- [ ] **Step 5: Test on mobile (or DevTools mobile emulation)**
  - Cards are tall enough to tap easily (min 76px) ✓
  - Tabs are tall enough (min 44px) ✓
  - No horizontal scroll ✓
  - Gradient header doesn't feel cramped ✓

- [ ] **Step 6: Final commit**

```bash
git add party.html
git commit -m "feat: party mode redesign complete — light theme, instructions, countdown, mobile"
```

---

## Self-Review Against Spec

| Spec Requirement                                       | Task                |
| ------------------------------------------------------ | ------------------- |
| All screens match game.html/index.html gradient header | Tasks 1, 5, 6, 7, 8 |
| Cream body (#f5f4f0) for all non-game screens          | Task 1              |
| Instructions in host lobby                             | Task 6              |
| Instructions in player lobby                           | Task 7              |
| Full-screen 3-2-1-GO countdown                         | Tasks 3, 9          |
| Countdown colors: white→teal→red→gold                  | Task 9              |
| Host game screen light theme                           | Tasks 4, 8          |
| Final screen light theme                               | Tasks 4, 8          |
| Chips light-themed (white bg, gray border)             | Task 2              |
| lb-row light-themed (white card)                       | Task 4              |
| Card min-height 76px                                   | Task 4              |
| Tab buttons min 44px                                   | Task 4              |
| touch-action: manipulation                             | Task 4              |
| Mobile media query                                     | Task 4              |
