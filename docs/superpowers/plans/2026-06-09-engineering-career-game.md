# Engineering Career Game Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Rebuild the existing `engineering_career_explorer.html` into a polished 3-page GitHub Pages site with a landing page, a game with score/streak/timer, and an informative career reference page.

**Architecture:** Three self-contained HTML files with inline `<style>` and `<script>`, no build step. Shared CSS variable palette and card data duplicated per file. `index.html` routes to `game.html` and `careers.html`.

**Tech Stack:** Vanilla HTML5/CSS3/JS, Tabler Icons CDN (v3.19.0).

---

## File Map

| File           | Responsibility                                                   |
| -------------- | ---------------------------------------------------------------- |
| `index.html`   | Landing page — two hero nav cards (Play / Explore)               |
| `game.html`    | Card-matching game with score, streak, timer, confetti, shake    |
| `careers.html` | Searchable reference of all 37 roles grouped by engineering type |

---

### Task 1: Create `index.html` — Landing Page

**Files:**

- Create: `index.html`

- [ ] **Step 1: Write the file**

Create `/Users/kuike/Desktop/UTEP_Manufacturing/CareerGame/index.html`:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Engineering Career Explorer</title>
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@3.19.0/dist/tabler-icons.min.css"
    />
    <style>
      *,
      *::before,
      *::after {
        box-sizing: border-box;
        margin: 0;
        padding: 0;
      }
      :root {
        --purple-50: #eeedfe;
        --purple-200: #afa9ec;
        --purple-400: #7f77dd;
        --purple-600: #534ab7;
        --purple-800: #3c3489;
        --purple-900: #26215c;
        --teal-50: #e1f5ee;
        --teal-200: #5dcaa5;
        --teal-600: #0f6e56;
        --teal-900: #04342c;
        --coral-50: #faece7;
        --coral-200: #f0997b;
        --coral-600: #993c1d;
        --gray-50: #f1efe8;
        --gray-100: #d3d1c7;
        --gray-400: #888780;
        --gray-600: #5f5e5a;
      }
      body {
        font-family:
          "Segoe UI",
          system-ui,
          -apple-system,
          sans-serif;
        background: #f5f4f0;
        min-height: 100vh;
        color: #2c2c2a;
        display: flex;
        flex-direction: column;
      }
      header {
        background: linear-gradient(
          135deg,
          #26215c 0%,
          #534ab7 60%,
          #0f6e56 100%
        );
        padding: 4rem 1rem 3rem;
        text-align: center;
        position: relative;
        overflow: hidden;
      }
      header::before {
        content: "";
        position: absolute;
        top: -60px;
        right: -60px;
        width: 260px;
        height: 260px;
        border-radius: 50%;
        background: rgba(255, 255, 255, 0.05);
      }
      header::after {
        content: "";
        position: absolute;
        bottom: -80px;
        left: -40px;
        width: 200px;
        height: 200px;
        border-radius: 50%;
        background: rgba(255, 255, 255, 0.04);
      }
      .header-badge {
        display: inline-flex;
        align-items: center;
        gap: 6px;
        background: rgba(255, 255, 255, 0.15);
        border: 1px solid rgba(255, 255, 255, 0.25);
        border-radius: 20px;
        padding: 4px 14px;
        font-size: 12px;
        font-weight: 500;
        color: rgba(255, 255, 255, 0.9);
        margin-bottom: 16px;
        letter-spacing: 0.04em;
        position: relative;
        z-index: 1;
      }
      header h1 {
        font-size: clamp(28px, 6vw, 46px);
        font-weight: 800;
        color: #fff;
        letter-spacing: -0.03em;
        margin-bottom: 12px;
        position: relative;
        z-index: 1;
      }
      header p {
        font-size: 16px;
        color: rgba(255, 255, 255, 0.75);
        max-width: 480px;
        margin: 0 auto;
        line-height: 1.6;
        position: relative;
        z-index: 1;
      }
      main {
        flex: 1;
        max-width: 860px;
        margin: 0 auto;
        padding: 3rem 1rem 2rem;
        width: 100%;
      }
      .section-label {
        text-align: center;
        font-size: 12px;
        font-weight: 600;
        color: var(--gray-400);
        letter-spacing: 0.1em;
        text-transform: uppercase;
        margin-bottom: 1.5rem;
      }
      .hero-cards {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 1.5rem;
      }
      @media (max-width: 560px) {
        .hero-cards {
          grid-template-columns: 1fr;
        }
      }
      .hero-card {
        display: flex;
        flex-direction: column;
        align-items: center;
        text-align: center;
        padding: 2.5rem 1.5rem 2rem;
        background: #fff;
        border: 2px solid #e0dfd8;
        border-radius: 20px;
        text-decoration: none;
        color: inherit;
        transition:
          transform 0.15s ease,
          box-shadow 0.15s ease,
          border-color 0.15s ease;
      }
      .hero-card:hover {
        transform: translateY(-6px);
        box-shadow: 0 16px 40px rgba(0, 0, 0, 0.1);
      }
      .hero-card.game-card {
        border-top: 4px solid var(--purple-600);
      }
      .hero-card.careers-card {
        border-top: 4px solid var(--teal-600);
      }
      .hero-card.game-card:hover {
        border-color: var(--purple-400);
        border-top-color: var(--purple-600);
      }
      .hero-card.careers-card:hover {
        border-color: var(--teal-200);
        border-top-color: var(--teal-600);
      }
      .hero-icon {
        width: 80px;
        height: 80px;
        border-radius: 20px;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 36px;
        margin-bottom: 1.25rem;
      }
      .game-card .hero-icon {
        background: var(--purple-50);
        color: var(--purple-600);
      }
      .careers-card .hero-icon {
        background: var(--teal-50);
        color: var(--teal-600);
      }
      .hero-card h2 {
        font-size: 22px;
        font-weight: 700;
        margin-bottom: 10px;
      }
      .hero-card p {
        font-size: 14px;
        color: var(--gray-600);
        line-height: 1.65;
        margin-bottom: 1.75rem;
        flex: 1;
      }
      .hero-btn {
        display: inline-flex;
        align-items: center;
        gap: 8px;
        padding: 11px 28px;
        border-radius: 10px;
        font-size: 14px;
        font-weight: 600;
        letter-spacing: 0.01em;
        transition: opacity 0.12s;
      }
      .game-card .hero-btn {
        background: var(--purple-600);
        color: #fff;
      }
      .careers-card .hero-btn {
        background: var(--teal-600);
        color: #fff;
      }
      .hero-card:hover .hero-btn {
        opacity: 0.85;
      }
      .stats-row {
        display: flex;
        justify-content: center;
        gap: 3rem;
        margin-top: 3rem;
        flex-wrap: wrap;
      }
      .stat {
        text-align: center;
      }
      .stat-num {
        font-size: 32px;
        font-weight: 800;
        color: var(--purple-600);
        line-height: 1;
      }
      .stat-lbl {
        font-size: 12px;
        color: var(--gray-400);
        margin-top: 6px;
      }
      footer {
        text-align: center;
        padding: 1.5rem;
        font-size: 12px;
        color: var(--gray-400);
        border-top: 1px solid var(--gray-100);
      }
    </style>
  </head>
  <body>
    <header>
      <div class="header-badge">
        <i class="ti ti-tool"></i> Engineering Outreach
      </div>
      <h1>🛠 Engineering Career Explorer</h1>
      <p>
        Discover the engineering role that matches your interests and passions
      </p>
    </header>
    <main>
      <div class="section-label">Choose your experience</div>
      <div class="hero-cards">
        <a href="game.html" class="hero-card game-card">
          <div class="hero-icon"><i class="ti ti-gamepad-2"></i></div>
          <h2>Play the Game</h2>
          <p>
            Make three choices — engineering type, industry, and problem to
            solve — and discover your career match. Track your score and streak!
          </p>
          <span class="hero-btn"
            ><i class="ti ti-player-play"></i> Let's Play</span
          >
        </a>
        <a href="careers.html" class="hero-card careers-card">
          <div class="hero-icon"><i class="ti ti-id-badge-2"></i></div>
          <h2>Explore Careers</h2>
          <p>
            Browse all 37 engineering roles with full descriptions, grouped by
            discipline. See what each role does and what combination reveals it.
          </p>
          <span class="hero-btn"><i class="ti ti-book-open"></i> Explore</span>
        </a>
      </div>
      <div class="stats-row">
        <div class="stat">
          <div class="stat-num">37</div>
          <div class="stat-lbl">Engineering Roles</div>
        </div>
        <div class="stat">
          <div class="stat-num">3</div>
          <div class="stat-lbl">Disciplines</div>
        </div>
        <div class="stat">
          <div class="stat-num">14</div>
          <div class="stat-lbl">Industries</div>
        </div>
      </div>
    </main>
    <footer>
      Engineering Career Explorer &nbsp;·&nbsp; Industrial, Manufacturing &amp;
      Systems Engineering
    </footer>
  </body>
</html>
```

- [ ] **Step 2: Verify in browser**

Open `index.html`. Confirm:

- Both hero cards render side by side (stacked on mobile)
- Hover lifts each card with shadow
- "37 / 3 / 14" stats row shows at the bottom
- Links resolve (404 expected until Tasks 2–3 are done)

---

### Task 2: Create `game.html` — The Game

**Files:**

- Create: `game.html`

- [ ] **Step 1: Write the file**

Create `/Users/kuike/Desktop/UTEP_Manufacturing/CareerGame/game.html`:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Play — Engineering Career Explorer</title>
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@3.19.0/dist/tabler-icons.min.css"
    />
    <style>
      *,
      *::before,
      *::after {
        box-sizing: border-box;
        margin: 0;
        padding: 0;
      }
      :root {
        --purple-50: #eeedfe;
        --purple-200: #afa9ec;
        --purple-400: #7f77dd;
        --purple-600: #534ab7;
        --purple-800: #3c3489;
        --purple-900: #26215c;
        --teal-50: #e1f5ee;
        --teal-200: #5dcaa5;
        --teal-600: #0f6e56;
        --teal-900: #04342c;
        --coral-50: #faece7;
        --coral-200: #f0997b;
        --coral-600: #993c1d;
        --coral-900: #4a1b0c;
        --amber-50: #faeeda;
        --amber-400: #ba7517;
        --gray-50: #f1efe8;
        --gray-100: #d3d1c7;
        --gray-400: #888780;
        --gray-600: #5f5e5a;
        --gray-800: #444441;
        --red-50: #fcebeb;
        --red-200: #f09595;
        --red-600: #a32d2d;
        --red-800: #791f1f;
        --green-50: #ecfdf5;
        --green-600: #059669;
      }
      body {
        font-family:
          "Segoe UI",
          system-ui,
          -apple-system,
          sans-serif;
        background: #f5f4f0;
        min-height: 100vh;
        color: #2c2c2a;
      }
      /* Confetti */
      #confetti-container {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        pointer-events: none;
        overflow: hidden;
        z-index: 9999;
      }
      .confetti-piece {
        position: absolute;
        top: -10px;
        animation: confettiFall linear forwards;
      }
      @keyframes confettiFall {
        0% {
          transform: translateY(0) rotate(0deg) scale(1);
          opacity: 1;
        }
        80% {
          opacity: 1;
        }
        100% {
          transform: translateY(100vh) rotate(720deg) scale(0.5);
          opacity: 0;
        }
      }
      /* Nav bar */
      .nav-bar {
        background: rgba(38, 33, 92, 0.97);
        padding: 10px 1.5rem;
        display: flex;
        align-items: center;
        justify-content: space-between;
        flex-wrap: wrap;
        gap: 8px;
        position: sticky;
        top: 0;
        z-index: 100;
      }
      .back-link {
        display: inline-flex;
        align-items: center;
        gap: 6px;
        color: rgba(255, 255, 255, 0.75);
        text-decoration: none;
        font-size: 13px;
        font-weight: 500;
        transition: color 0.12s;
      }
      .back-link:hover {
        color: #fff;
      }
      .game-stats {
        display: flex;
        align-items: center;
        gap: 8px;
        flex-wrap: wrap;
      }
      .stat-chip {
        display: inline-flex;
        align-items: center;
        gap: 5px;
        padding: 5px 12px;
        border-radius: 20px;
        font-size: 12px;
        font-weight: 600;
        letter-spacing: 0.02em;
      }
      .score-chip {
        background: rgba(83, 74, 183, 0.35);
        color: #afa9ec;
        border: 1px solid rgba(83, 74, 183, 0.5);
      }
      .streak-chip {
        background: rgba(186, 117, 23, 0.25);
        color: #f5c67a;
        border: 1px solid rgba(186, 117, 23, 0.4);
      }
      .timer-chip {
        background: rgba(15, 110, 86, 0.25);
        color: #5dcaa5;
        border: 1px solid rgba(15, 110, 86, 0.4);
      }
      /* Header */
      header {
        background: linear-gradient(
          135deg,
          #26215c 0%,
          #534ab7 60%,
          #0f6e56 100%
        );
        padding: 2rem 1rem 2.5rem;
        text-align: center;
        position: relative;
        overflow: hidden;
      }
      header::before {
        content: "";
        position: absolute;
        top: -40px;
        right: -40px;
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
        padding: 4px 14px;
        font-size: 12px;
        font-weight: 500;
        color: rgba(255, 255, 255, 0.9);
        margin-bottom: 12px;
        letter-spacing: 0.04em;
        position: relative;
        z-index: 1;
      }
      header h1 {
        font-size: clamp(20px, 5vw, 30px);
        font-weight: 700;
        color: #fff;
        letter-spacing: -0.02em;
        margin-bottom: 8px;
        position: relative;
        z-index: 1;
      }
      header p {
        font-size: 14px;
        color: rgba(255, 255, 255, 0.75);
        position: relative;
        z-index: 1;
      }
      /* Main */
      .main {
        max-width: 960px;
        margin: 0 auto;
        padding: 1.5rem 1rem 3rem;
      }
      /* Progress */
      .counter {
        text-align: right;
        font-size: 12px;
        color: var(--gray-400);
        margin-bottom: 4px;
      }
      .progress-bar {
        height: 5px;
        border-radius: 4px;
        background: var(--gray-100);
        overflow: hidden;
        margin-bottom: 1.5rem;
      }
      .progress-fill {
        height: 100%;
        width: 0%;
        background: linear-gradient(
          90deg,
          var(--purple-600),
          var(--teal-600),
          var(--coral-600)
        );
        border-radius: 4px;
        transition: width 0.35s ease;
      }
      /* Step headers */
      .step-header {
        display: flex;
        align-items: center;
        gap: 10px;
        margin-bottom: 12px;
        margin-top: 2rem;
      }
      .step-num {
        width: 28px;
        height: 28px;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 13px;
        font-weight: 700;
        flex-shrink: 0;
      }
      .step-eng .step-num {
        background: var(--purple-600);
        color: #fff;
      }
      .step-ind .step-num {
        background: var(--teal-600);
        color: #fff;
      }
      .step-prod .step-num {
        background: var(--coral-600);
        color: #fff;
      }
      .step-header h2 {
        font-size: 16px;
        font-weight: 600;
        color: #2c2c2a;
      }
      .step-header span {
        font-size: 13px;
        color: var(--gray-600);
        font-weight: 400;
      }
      /* Cards */
      .cards-grid {
        display: flex;
        flex-wrap: wrap;
        gap: 8px;
      }
      .card {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        gap: 7px;
        padding: 12px 8px;
        width: 104px;
        min-height: 88px;
        background: #fff;
        border: 1.5px solid #e0dfd8;
        border-radius: 12px;
        cursor: pointer;
        transition:
          transform 0.12s ease,
          border-color 0.12s ease,
          background 0.12s ease,
          box-shadow 0.12s ease;
        user-select: none;
        opacity: 0;
        animation: cardIn 0.35s ease forwards;
      }
      @keyframes cardIn {
        from {
          opacity: 0;
          transform: translateY(10px);
        }
        to {
          opacity: 1;
          transform: translateY(0);
        }
      }
      .card:hover {
        transform: translateY(-3px);
        box-shadow: 0 6px 16px rgba(0, 0, 0, 0.08);
      }
      .card:active {
        transform: translateY(0);
      }
      .card i {
        font-size: 24px;
        transition: transform 0.15s;
      }
      .card:hover i {
        transform: scale(1.12);
      }
      .card span {
        font-size: 11px;
        font-weight: 500;
        text-align: center;
        line-height: 1.35;
        color: #444441;
      }
      .card-eng i {
        color: var(--purple-600);
      }
      .card-ind i {
        color: var(--teal-600);
      }
      .card-prod i {
        color: var(--coral-600);
      }
      .card.sel-eng {
        background: var(--purple-50);
        border-color: var(--purple-400);
        border-width: 2px;
      }
      .card.sel-eng span {
        color: var(--purple-800);
      }
      .card.sel-ind {
        background: var(--teal-50);
        border-color: var(--teal-200);
        border-width: 2px;
      }
      .card.sel-ind span {
        color: var(--teal-600);
      }
      .card.sel-prod {
        background: var(--coral-50);
        border-color: var(--coral-200);
        border-width: 2px;
      }
      .card.sel-prod span {
        color: var(--coral-600);
      }
      /* Divider */
      .divider {
        height: 1px;
        background: var(--gray-100);
        margin: 2rem 0;
      }
      /* Slots */
      .selection-label {
        text-align: center;
        font-size: 13px;
        font-weight: 600;
        color: var(--gray-600);
        letter-spacing: 0.06em;
        text-transform: uppercase;
        margin-bottom: 1rem;
      }
      .slots-row {
        display: flex;
        align-items: center;
        gap: 8px;
        flex-wrap: wrap;
        justify-content: center;
        transition: transform 0.1s;
      }
      @keyframes shake {
        0%,
        100% {
          transform: translateX(0);
        }
        15% {
          transform: translateX(-8px);
        }
        30% {
          transform: translateX(8px);
        }
        45% {
          transform: translateX(-6px);
        }
        60% {
          transform: translateX(6px);
        }
        75% {
          transform: translateX(-3px);
        }
        90% {
          transform: translateX(3px);
        }
      }
      .slots-row.shaking {
        animation: shake 0.45s ease;
      }
      .slot {
        flex: 1;
        min-width: 200px;
        max-width: 260px;
        border: 2px dashed var(--gray-100);
        border-radius: 14px;
        padding: 16px 12px;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        gap: 6px;
        min-height: 100px;
        background: #faf9f7;
        transition: all 0.2s ease;
      }
      .slot-lbl {
        font-size: 10px;
        font-weight: 600;
        letter-spacing: 0.08em;
        text-transform: uppercase;
        color: var(--gray-400);
      }
      .slot i {
        font-size: 26px;
      }
      .slot-eng-color i {
        color: var(--purple-400);
      }
      .slot-ind-color i {
        color: var(--teal-200);
      }
      .slot-prod-color i {
        color: var(--coral-200);
      }
      .slot-name {
        font-size: 12px;
        font-weight: 500;
        text-align: center;
        color: var(--gray-600);
      }
      .slot-empty {
        font-size: 12px;
        color: var(--gray-400);
        font-style: italic;
      }
      .slot.filled-eng {
        background: var(--purple-50);
        border-style: solid;
        border-color: var(--purple-400);
      }
      .slot.filled-eng .slot-lbl {
        color: var(--purple-600);
      }
      .slot.filled-eng .slot-name {
        color: var(--purple-800);
      }
      .slot.filled-eng i {
        color: var(--purple-600);
      }
      .slot.filled-ind {
        background: var(--teal-50);
        border-style: solid;
        border-color: var(--teal-200);
      }
      .slot.filled-ind .slot-lbl {
        color: var(--teal-600);
      }
      .slot.filled-ind .slot-name {
        color: var(--teal-600);
      }
      .slot.filled-ind i {
        color: var(--teal-600);
      }
      .slot.filled-prod {
        background: var(--coral-50);
        border-style: solid;
        border-color: var(--coral-200);
      }
      .slot.filled-prod .slot-lbl {
        color: var(--coral-600);
      }
      .slot.filled-prod .slot-name {
        color: var(--coral-600);
      }
      .slot.filled-prod i {
        color: var(--coral-600);
      }
      .plus {
        font-size: 22px;
        color: var(--gray-400);
        flex-shrink: 0;
        font-weight: 300;
      }
      /* Result */
      .result-area {
        display: none;
        margin-top: 1.5rem;
        border-radius: 16px;
        overflow: hidden;
      }
      .result-success {
        background: linear-gradient(
          135deg,
          var(--purple-50) 0%,
          var(--teal-50) 100%
        );
        border: 2px solid var(--purple-200);
        padding: 2rem 1.5rem;
        text-align: center;
        animation: popIn 0.3s ease;
      }
      .result-fail {
        background: var(--red-50);
        border: 2px solid var(--red-200);
        padding: 2rem 1.5rem;
        text-align: center;
        animation: popIn 0.3s ease;
      }
      @keyframes popIn {
        from {
          opacity: 0;
          transform: scale(0.96) translateY(8px);
        }
        to {
          opacity: 1;
          transform: scale(1) translateY(0);
        }
      }
      .result-badge {
        display: inline-flex;
        align-items: center;
        gap: 6px;
        background: var(--purple-600);
        color: #fff;
        border-radius: 20px;
        padding: 5px 16px;
        font-size: 12px;
        font-weight: 600;
        letter-spacing: 0.04em;
        margin-bottom: 14px;
      }
      .result-time-badge {
        display: inline-flex;
        align-items: center;
        gap: 5px;
        background: var(--green-50);
        color: var(--green-600);
        border: 1px solid var(--green-600);
        border-radius: 20px;
        padding: 3px 12px;
        font-size: 11px;
        font-weight: 600;
        margin-left: 8px;
      }
      .result-role-label {
        font-size: 11px;
        font-weight: 600;
        text-transform: uppercase;
        letter-spacing: 0.1em;
        color: var(--purple-400);
        margin-bottom: 8px;
      }
      .result-role {
        font-size: clamp(18px, 4vw, 24px);
        font-weight: 700;
        color: var(--purple-900);
        margin-bottom: 12px;
      }
      .result-desc {
        font-size: 14px;
        color: var(--purple-800);
        line-height: 1.7;
        max-width: 540px;
        margin: 0 auto;
      }
      .result-x {
        font-size: 52px;
        line-height: 1;
        color: #e24b4a;
        margin-bottom: 10px;
        font-weight: 700;
      }
      .result-fail-msg {
        font-size: 16px;
        font-weight: 600;
        color: var(--red-800);
        margin-bottom: 6px;
      }
      .result-fail-sub {
        font-size: 13px;
        color: var(--red-600);
      }
      /* Actions */
      .actions {
        display: flex;
        gap: 10px;
        justify-content: center;
        margin-top: 1.5rem;
        flex-wrap: wrap;
      }
      .btn-try {
        padding: 10px 22px;
        font-size: 13px;
        font-weight: 600;
        border-radius: 8px;
        border: none;
        background: var(--purple-600);
        color: #fff;
        cursor: pointer;
        transition: all 0.12s;
        display: flex;
        align-items: center;
        gap: 6px;
      }
      .btn-try:hover {
        background: var(--purple-800);
      }
      .btn-reset {
        padding: 10px 22px;
        font-size: 13px;
        font-weight: 500;
        border-radius: 8px;
        border: 1.5px solid var(--gray-100);
        background: #fff;
        color: var(--gray-600);
        cursor: pointer;
        transition: all 0.12s;
        display: flex;
        align-items: center;
        gap: 6px;
      }
      .btn-reset:hover {
        background: var(--gray-50);
      }
      footer {
        text-align: center;
        padding: 1.5rem;
        font-size: 12px;
        color: var(--gray-400);
        border-top: 1px solid var(--gray-100);
      }
      @media (max-width: 560px) {
        .card {
          width: 88px;
          min-height: 80px;
        }
        .slot {
          min-width: 160px;
        }
        .plus {
          display: none;
        }
        .nav-bar {
          flex-direction: column;
          align-items: flex-start;
        }
      }
    </style>
  </head>
  <body>
    <div id="confetti-container"></div>

    <nav class="nav-bar">
      <a href="index.html" class="back-link"
        ><i class="ti ti-arrow-left"></i> Home</a
      >
      <div class="game-stats">
        <div class="stat-chip score-chip">
          <i class="ti ti-trophy"></i> <span id="score">0</span> matches
        </div>
        <div class="stat-chip streak-chip">
          🔥 <span id="streak">0</span> streak
        </div>
        <div class="stat-chip timer-chip">
          <i class="ti ti-clock"></i> Best: <span id="best-time">--</span>
        </div>
      </div>
    </nav>

    <header>
      <div class="header-badge">
        <i class="ti ti-tool"></i> Engineering Outreach
      </div>
      <h1>🛠 Engineering Career Explorer</h1>
      <p>Select one card from each section to discover your engineering role</p>
    </header>

    <main class="main">
      <div class="counter" id="counter">0 of 3 selected</div>
      <div class="progress-bar">
        <div class="progress-fill" id="progress"></div>
      </div>

      <div class="step-header step-eng">
        <div class="step-num">1</div>
        <h2>Type of Engineering <span>— Choose one</span></h2>
      </div>
      <div class="cards-grid" id="eng-cards"></div>

      <div class="divider"></div>

      <div class="step-header step-ind">
        <div class="step-num">2</div>
        <h2>Workforce / Industry <span>— Choose one</span></h2>
      </div>
      <div class="cards-grid" id="ind-cards"></div>

      <div class="divider"></div>

      <div class="step-header step-prod">
        <div class="step-num">3</div>
        <h2>Product / Problem to Solve <span>— Choose one</span></h2>
      </div>
      <div class="cards-grid" id="prod-cards"></div>

      <div class="divider"></div>

      <div class="selection-label">Your Selections</div>
      <div class="slots-row" id="slots-row">
        <div class="slot slot-eng-color" id="slot-eng">
          <div class="slot-lbl">Engineering Type</div>
          <i class="ti ti-settings" id="slot-eng-icon"></i>
          <div class="slot-empty" id="slot-eng-text">Select above ↑</div>
        </div>
        <div class="plus">+</div>
        <div class="slot slot-ind-color" id="slot-ind">
          <div class="slot-lbl">Industry</div>
          <i class="ti ti-building-factory" id="slot-ind-icon"></i>
          <div class="slot-empty" id="slot-ind-text">Select above ↑</div>
        </div>
        <div class="plus">+</div>
        <div class="slot slot-prod-color" id="slot-prod">
          <div class="slot-lbl">Product / Problem</div>
          <i class="ti ti-bulb" id="slot-prod-icon"></i>
          <div class="slot-empty" id="slot-prod-text">Select above ↑</div>
        </div>
      </div>

      <div class="result-area" id="result-area"></div>

      <div class="actions">
        <button
          class="btn-try"
          id="btn-try"
          onclick="clearSelections()"
          style="display:none"
        >
          <i class="ti ti-refresh"></i> Try Another Combo
        </button>
        <button class="btn-reset" onclick="resetAll()">
          <i class="ti ti-trash"></i> Reset All
        </button>
      </div>
    </main>

    <footer>
      Engineering Career Explorer &nbsp;·&nbsp; Industrial, Manufacturing &amp;
      Systems Engineering
    </footer>

    <script>
      const engData = [
        { id: "IE", label: "Industrial Engineering", icon: "ti-chart-arrows" },
        { id: "ME", label: "Manufacturing Engineering", icon: "ti-assembly" },
        { id: "SE", label: "Systems Engineering", icon: "ti-sitemap" },
      ];

      const indData = [
        { id: "hospital", label: "Hospitals", icon: "ti-building-hospital" },
        { id: "factory", label: "Factories", icon: "ti-building-factory" },
        { id: "aerospace", label: "Aerospace", icon: "ti-rocket" },
        { id: "logistics", label: "Logistics", icon: "ti-truck" },
        { id: "bizops", label: "Business Operations", icon: "ti-briefcase" },
        {
          id: "healthcare",
          label: "Healthcare",
          icon: "ti-heart-rate-monitor",
        },
        {
          id: "supplychain",
          label: "Supply Chain",
          icon: "ti-arrows-exchange",
        },
        { id: "technology", label: "Technology", icon: "ti-cpu" },
        { id: "energy", label: "Energy", icon: "ti-bolt" },
        { id: "automotive", label: "Automotive", icon: "ti-car" },
        { id: "construction", label: "Construction", icon: "ti-crane" },
        { id: "warehouse", label: "Warehouses", icon: "ti-building-warehouse" },
        { id: "airport", label: "Airports", icon: "ti-plane" },
        { id: "food", label: "Food Production", icon: "ti-salad" },
      ];

      const prodData = [
        { id: "computers", label: "Computers", icon: "ti-device-desktop" },
        {
          id: "floorlayout",
          label: "Production Floor Layout",
          icon: "ti-layout-dashboard",
        },
        { id: "quality", label: "Quality Control", icon: "ti-certificate" },
        { id: "costs", label: "Reducing Costs", icon: "ti-coins" },
        { id: "safety", label: "Improving Safety", icon: "ti-shield-check" },
        {
          id: "productivity",
          label: "Increasing Productivity",
          icon: "ti-trending-up",
        },
        { id: "data", label: "Data Analysis", icon: "ti-chart-bar" },
        { id: "inventory", label: "Inventory Management", icon: "ti-package" },
        { id: "process", label: "Process Improvement", icon: "ti-refresh" },
        { id: "medical", label: "Medical Devices", icon: "ti-stethoscope" },
        { id: "delivery", label: "Faster Delivery", icon: "ti-clock-bolt" },
        {
          id: "customer",
          label: "Better Customer Service",
          icon: "ti-mood-happy",
        },
        { id: "robotics", label: "Robotics / Automation", icon: "ti-robot" },
        { id: "waste", label: "Waste Reduction", icon: "ti-recycle" },
        {
          id: "scheduling",
          label: "Scheduling Workers",
          icon: "ti-calendar-time",
        },
        {
          id: "patientflow",
          label: "Improving Patient Flow",
          icon: "ti-activity",
        },
      ];

      const roleMap = {
        "IE|hospital|patientflow": {
          role: "Healthcare Systems Engineer",
          desc: "A Healthcare Systems Engineer applies industrial engineering methods to hospitals and clinics — improving patient flow, reducing wait times, and making care delivery safer and more efficient.",
        },
        "IE|factory|costs": {
          role: "Process Improvement Engineer",
          desc: "A Process Improvement Engineer identifies waste and inefficiencies in manufacturing or business operations and redesigns processes to cut costs and boost performance.",
        },
        "ME|factory|floorlayout": {
          role: "Manufacturing Engineer",
          desc: "A Manufacturing Engineer designs and optimizes factory floor layouts, production lines, and assembly systems to maximize output and quality.",
        },
        "ME|food|quality": {
          role: "Quality Engineer",
          desc: "A Quality Engineer ensures products, processes, and systems meet strict standards — reducing defects, minimizing errors, and improving customer satisfaction.",
        },
        "SE|aerospace|computers": {
          role: "Aerospace Systems Engineer",
          desc: "An Aerospace Systems Engineer integrates avionics, software, and hardware subsystems in aircraft or spacecraft to ensure mission success and system reliability.",
        },
        "IE|supplychain|delivery": {
          role: "Supply Chain Engineer",
          desc: "A Supply Chain Engineer designs and optimizes the flow of goods, information, and resources across a supply chain to ensure products arrive faster and at lower cost.",
        },
        "IE|warehouse|inventory": {
          role: "Logistics Engineer",
          desc: "A Logistics Engineer plans and improves warehouse layouts, inventory systems, and distribution networks to move products efficiently and accurately.",
        },
        "ME|factory|robotics": {
          role: "Automation Engineer",
          desc: "An Automation Engineer designs and implements robotic systems, automated machinery, and smart manufacturing solutions that increase speed, precision, and safety.",
        },
        "SE|technology|computers": {
          role: "Technology Systems Analyst",
          desc: "A Technology Systems Analyst designs, integrates, and optimizes complex software and hardware systems to meet the evolving needs of tech-driven organizations.",
        },
        "IE|bizops|data": {
          role: "Operations Analyst",
          desc: "An Operations Analyst uses data analysis, modeling, and optimization techniques to improve business processes, reduce costs, and support strategic decision-making.",
        },
        "IE|factory|process": {
          role: "Process Engineer",
          desc: "A Process Engineer analyzes and redesigns manufacturing or business workflows to eliminate bottlenecks, reduce variation, and improve overall throughput and quality.",
        },
        "IE|factory|safety": {
          role: "Safety & Industrial Engineer",
          desc: "A Safety & Industrial Engineer combines ergonomics, process design, and risk analysis to build safer workplaces while maintaining productivity.",
        },
        "IE|factory|productivity": {
          role: "Industrial Engineer",
          desc: "An Industrial Engineer optimizes systems of people, machines, and materials to maximize productivity and eliminate waste across manufacturing and service environments.",
        },
        "ME|automotive|robotics": {
          role: "Automotive Automation Engineer",
          desc: "An Automotive Automation Engineer develops robotic assembly lines and smart manufacturing systems for vehicle production, improving speed, precision, and safety.",
        },
        "ME|automotive|quality": {
          role: "Automotive Quality Engineer",
          desc: "An Automotive Quality Engineer ensures vehicles and components meet rigorous safety and performance standards throughout design, production, and delivery.",
        },
        "SE|energy|computers": {
          role: "Energy Systems Engineer",
          desc: "An Energy Systems Engineer designs and manages integrated power systems — from smart grids to renewable energy infrastructure — ensuring reliable and efficient energy delivery.",
        },
        "IE|hospital|costs": {
          role: "Healthcare Operations Engineer",
          desc: "A Healthcare Operations Engineer applies lean and industrial engineering principles to reduce costs in hospitals while maintaining high-quality patient care.",
        },
        "IE|logistics|delivery": {
          role: "Logistics & Distribution Engineer",
          desc: "A Logistics & Distribution Engineer designs transportation networks and fulfillment systems to ensure goods reach customers faster and at lower operational cost.",
        },
        "SE|airport|scheduling": {
          role: "Airport Systems Engineer",
          desc: "An Airport Systems Engineer plans and optimizes complex airport operations — from gate scheduling and baggage systems to security flow and control systems integration.",
        },
        "IE|factory|scheduling": {
          role: "Production Planning Engineer",
          desc: "A Production Planning Engineer schedules workers, machines, and materials on the factory floor to meet demand, reduce downtime, and optimize shift efficiency.",
        },
        "ME|food|robotics": {
          role: "Food Automation Engineer",
          desc: "A Food Automation Engineer designs automated processing and packaging systems in food manufacturing to improve throughput, hygiene, and consistency.",
        },
        "SE|technology|data": {
          role: "Data Systems Engineer",
          desc: "A Data Systems Engineer architects and integrates large-scale data platforms, pipelines, and analytics infrastructure to support intelligent decision-making.",
        },
        "IE|healthcare|medical": {
          role: "Biomedical & Industrial Engineer",
          desc: "This engineer combines industrial engineering methods with healthcare expertise to optimize the deployment, maintenance, and workflow around medical devices and clinical equipment.",
        },
        "ME|factory|waste": {
          role: "Lean Manufacturing Engineer",
          desc: "A Lean Manufacturing Engineer applies lean and Six Sigma principles to eliminate waste — overproduction, idle time, defects — and create more value with fewer resources.",
        },
        "IE|bizops|customer": {
          role: "Customer Experience & Operations Engineer",
          desc: "This engineer uses operations research and process improvement methods to redesign service systems, reduce friction, and elevate the customer experience.",
        },
        "IE|construction|safety": {
          role: "Construction Safety Engineer",
          desc: "This engineer designs safe construction workflows, site layouts, and worker scheduling systems to reduce accidents and keep projects on time and on budget.",
        },
        "SE|aerospace|safety": {
          role: "Systems Safety Engineer",
          desc: "A Systems Safety Engineer identifies and mitigates risks in complex aerospace and defense systems, ensuring every subsystem meets strict reliability and safety requirements.",
        },
        "ME|healthcare|medical": {
          role: "Medical Device Manufacturing Engineer",
          desc: "A Medical Device Manufacturing Engineer designs and validates production processes for medical equipment, ensuring devices are safe, effective, and compliant with regulatory standards.",
        },
        "SE|energy|data": {
          role: "Energy Data Systems Engineer",
          desc: "An Energy Data Systems Engineer builds integrated systems that collect, process, and visualize power grid and energy consumption data to drive smarter energy management decisions.",
        },
        "IE|food|quality": {
          role: "Food Quality & Industrial Engineer",
          desc: "This engineer applies quality control methods and lean principles to food production lines, ensuring consistent product standards and regulatory compliance.",
        },
        "IE|airport|scheduling": {
          role: "Airport Operations & Industrial Engineer",
          desc: "This engineer uses industrial engineering tools to optimize staffing, gate assignments, and terminal flow at airports, reducing delays and improving passenger experience.",
        },
        "SE|construction|safety": {
          role: "Construction Systems Engineer",
          desc: "A Construction Systems Engineer integrates project planning, safety systems, and structural requirements to ensure complex construction projects are delivered safely and on schedule.",
        },
        "ME|energy|robotics": {
          role: "Energy Automation Engineer",
          desc: "An Energy Automation Engineer designs automated systems for power plants and renewable energy installations, improving efficiency, safety, and uptime.",
        },
        "IE|warehouse|scheduling": {
          role: "Warehouse Operations Engineer",
          desc: "A Warehouse Operations Engineer optimizes worker schedules, picking routes, and storage layouts to maximize throughput and minimize operational cost.",
        },
        "SE|technology|process": {
          role: "IT Process Systems Engineer",
          desc: "An IT Process Systems Engineer designs and integrates enterprise software workflows, ensuring that IT systems and business processes are aligned, scalable, and efficient.",
        },
        "IE|supplychain|inventory": {
          role: "Supply Chain & Inventory Engineer",
          desc: "This engineer develops inventory management strategies and supply chain models that balance stock levels, reduce holding costs, and prevent stockouts across distribution networks.",
        },
        "ME|construction|floorlayout": {
          role: "Construction Manufacturing Engineer",
          desc: "A Construction Manufacturing Engineer designs prefabrication layouts and modular construction workflows, bringing manufacturing efficiency to building and infrastructure projects.",
        },
      };

      // Game state
      let sel = { eng: null, ind: null, prod: null };
      let score = 0;
      let streak = 0;
      let bestTime = null;
      let roundStart = null;
      let resultShown = false;

      function renderCards(data, containerId, type) {
        const wrap = document.getElementById(containerId);
        data.forEach((item, i) => {
          const c = document.createElement("div");
          c.className = `card card-${type}`;
          c.dataset.id = item.id;
          c.style.animationDelay = `${i * 40}ms`;
          c.innerHTML = `<i class="ti ${item.icon}" aria-hidden="true"></i><span>${item.label}</span>`;
          c.addEventListener("click", () => selectCard(type, item, c));
          wrap.appendChild(c);
        });
      }

      function selectCard(type, item, el) {
        if (resultShown) return;
        if (roundStart === null) roundStart = Date.now();
        document.querySelectorAll(`.card-${type}`).forEach((c) => {
          c.classList.remove("sel-eng", "sel-ind", "sel-prod");
        });
        el.classList.add(`sel-${type}`);
        sel[type] = item;
        updateSlot(type, item);
        updateProgress();
        checkResult();
      }

      function updateSlot(type, item) {
        const slot = document.getElementById(`slot-${type}`);
        const iconEl = document.getElementById(`slot-${type}-icon`);
        const textEl = document.getElementById(`slot-${type}-text`);
        const classes = {
          eng: "filled-eng",
          ind: "filled-ind",
          prod: "filled-prod",
        };
        slot.className = `slot ${classes[type]}`;
        iconEl.className = `ti ${item.icon}`;
        textEl.className = "slot-name";
        textEl.textContent = item.label;
      }

      function updateProgress() {
        const count = [sel.eng, sel.ind, sel.prod].filter(Boolean).length;
        document.getElementById("progress").style.width =
          (count / 3) * 100 + "%";
        document.getElementById("counter").textContent =
          `${count} of 3 selected`;
      }

      function checkResult() {
        if (!sel.eng || !sel.ind || !sel.prod) return;
        resultShown = true;
        const key = `${sel.eng.id}|${sel.ind.id}|${sel.prod.id}`;
        const match = roleMap[key];
        const area = document.getElementById("result-area");
        area.style.display = "block";

        if (match) {
          const elapsed = Math.round((Date.now() - roundStart) / 1000);
          const isBest = bestTime === null || elapsed < bestTime;
          if (isBest) {
            bestTime = elapsed;
            document.getElementById("best-time").textContent = `${bestTime}s`;
          }
          score++;
          streak++;
          document.getElementById("score").textContent = score;
          document.getElementById("streak").textContent = streak;
          area.className = "result-area result-success";
          area.innerHTML = `
      <div>
        <div class="result-badge"><i class="ti ti-trophy"></i> Match found!</div>
        ${isBest ? '<span class="result-time-badge">⚡ New best: ' + elapsed + "s</span>" : '<span class="result-time-badge">⏱ ' + elapsed + "s</span>"}
      </div>
      <div class="result-role-label" style="margin-top:14px">Your Engineering Role Is</div>
      <div class="result-role">${match.role}</div>
      <div class="result-desc">${match.desc}</div>`;
          launchConfetti();
        } else {
          streak = 0;
          document.getElementById("streak").textContent = streak;
          area.className = "result-area result-fail";
          area.innerHTML = `
      <div class="result-x">✗</div>
      <div class="result-fail-msg">This combination doesn't match a real engineering role.</div>
      <div class="result-fail-sub">Try a different combination — there are many paths to discover!</div>`;
          const row = document.getElementById("slots-row");
          row.classList.add("shaking");
          setTimeout(() => row.classList.remove("shaking"), 500);
        }

        document.getElementById("btn-try").style.display = "inline-flex";
        area.scrollIntoView({ behavior: "smooth", block: "nearest" });
      }

      function clearSelections() {
        sel = { eng: null, ind: null, prod: null };
        resultShown = false;
        roundStart = null;
        document.querySelectorAll(".card").forEach((c) => {
          c.classList.remove("sel-eng", "sel-ind", "sel-prod");
        });
        ["eng", "ind", "prod"].forEach((type) => {
          const slot = document.getElementById(`slot-${type}`);
          const icons = {
            eng: "ti-settings",
            ind: "ti-building-factory",
            prod: "ti-bulb",
          };
          const colorClass = {
            eng: "slot-eng-color",
            ind: "slot-ind-color",
            prod: "slot-prod-color",
          };
          slot.className = `slot ${colorClass[type]}`;
          document.getElementById(`slot-${type}-icon`).className =
            `ti ${icons[type]}`;
          const textEl = document.getElementById(`slot-${type}-text`);
          textEl.className = "slot-empty";
          textEl.textContent = "Select above ↑";
        });
        const area = document.getElementById("result-area");
        area.style.display = "none";
        area.innerHTML = "";
        document.getElementById("btn-try").style.display = "none";
        updateProgress();
      }

      function resetAll() {
        score = 0;
        streak = 0;
        bestTime = null;
        document.getElementById("score").textContent = "0";
        document.getElementById("streak").textContent = "0";
        document.getElementById("best-time").textContent = "--";
        clearSelections();
      }

      function launchConfetti() {
        const colors = [
          "#534AB7",
          "#0F6E56",
          "#993C1D",
          "#BA7517",
          "#5DCAA5",
          "#F0997B",
          "#7F77DD",
          "#AFA9EC",
        ];
        const container = document.getElementById("confetti-container");
        for (let i = 0; i < 70; i++) {
          const el = document.createElement("div");
          el.className = "confetti-piece";
          const size = 6 + Math.random() * 8;
          el.style.cssText = `
      left: ${10 + Math.random() * 80}%;
      background: ${colors[Math.floor(Math.random() * colors.length)]};
      animation-delay: ${Math.random() * 0.6}s;
      animation-duration: ${0.9 + Math.random() * 0.8}s;
      width: ${size}px;
      height: ${size}px;
      border-radius: ${Math.random() > 0.5 ? "50%" : "3px"};
    `;
          container.appendChild(el);
          setTimeout(() => el.remove(), 2000);
        }
      }

      renderCards(engData, "eng-cards", "eng");
      renderCards(indData, "ind-cards", "ind");
      renderCards(prodData, "prod-cards", "prod");
    </script>
  </body>
</html>
```

- [ ] **Step 2: Verify in browser**

Open `game.html`. Confirm:

- Nav bar shows "0 matches / 0 streak / Best: --"
- Cards animate in with staggered fade-up
- Selecting a card highlights it and updates the slot
- Progress bar fills as selections are made
- Selecting all 3 for a known match (e.g. IE → Factories → Reducing Costs) triggers confetti and shows "Process Improvement Engineer"
- Score increments to 1, streak to 1, best time shows elapsed seconds
- Selecting all 3 for an unknown combo shakes the slots row and shows the red fail card, streak resets to 0
- "Try Another Combo" button clears selections but keeps score/streak/timer
- "Reset All" clears everything including score/streak/timer

---

### Task 3: Create `careers.html` — Career Reference

**Files:**

- Create: `careers.html`

- [ ] **Step 1: Write the file**

Create `/Users/kuike/Desktop/UTEP_Manufacturing/CareerGame/careers.html`:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Explore Careers — Engineering Career Explorer</title>
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@3.19.0/dist/tabler-icons.min.css"
    />
    <style>
      *,
      *::before,
      *::after {
        box-sizing: border-box;
        margin: 0;
        padding: 0;
      }
      :root {
        --purple-50: #eeedfe;
        --purple-200: #afa9ec;
        --purple-400: #7f77dd;
        --purple-600: #534ab7;
        --purple-800: #3c3489;
        --purple-900: #26215c;
        --teal-50: #e1f5ee;
        --teal-200: #5dcaa5;
        --teal-600: #0f6e56;
        --teal-900: #04342c;
        --coral-50: #faece7;
        --coral-200: #f0997b;
        --coral-600: #993c1d;
        --coral-900: #4a1b0c;
        --amber-50: #faeeda;
        --amber-400: #ba7517;
        --gray-50: #f1efe8;
        --gray-100: #d3d1c7;
        --gray-400: #888780;
        --gray-600: #5f5e5a;
        --gray-800: #444441;
      }
      body {
        font-family:
          "Segoe UI",
          system-ui,
          -apple-system,
          sans-serif;
        background: #f5f4f0;
        min-height: 100vh;
        color: #2c2c2a;
      }
      .nav-bar {
        background: rgba(38, 33, 92, 0.97);
        padding: 10px 1.5rem;
        display: flex;
        align-items: center;
        justify-content: space-between;
        position: sticky;
        top: 0;
        z-index: 100;
      }
      .back-link {
        display: inline-flex;
        align-items: center;
        gap: 6px;
        color: rgba(255, 255, 255, 0.75);
        text-decoration: none;
        font-size: 13px;
        font-weight: 500;
        transition: color 0.12s;
      }
      .back-link:hover {
        color: #fff;
      }
      .nav-count {
        font-size: 12px;
        color: rgba(255, 255, 255, 0.5);
      }
      header {
        background: linear-gradient(135deg, #26215c 0%, #0f6e56 100%);
        padding: 2.5rem 1rem 2rem;
        text-align: center;
        position: relative;
        overflow: hidden;
      }
      header::before {
        content: "";
        position: absolute;
        top: -40px;
        right: -40px;
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
        padding: 4px 14px;
        font-size: 12px;
        font-weight: 500;
        color: rgba(255, 255, 255, 0.9);
        margin-bottom: 12px;
        letter-spacing: 0.04em;
        position: relative;
        z-index: 1;
      }
      header h1 {
        font-size: clamp(20px, 5vw, 30px);
        font-weight: 700;
        color: #fff;
        letter-spacing: -0.02em;
        margin-bottom: 8px;
        position: relative;
        z-index: 1;
      }
      header p {
        font-size: 14px;
        color: rgba(255, 255, 255, 0.75);
        position: relative;
        z-index: 1;
      }
      .main {
        max-width: 1000px;
        margin: 0 auto;
        padding: 2rem 1rem 3rem;
      }
      /* Search */
      .search-wrap {
        position: relative;
        margin-bottom: 2rem;
      }
      .search-wrap i {
        position: absolute;
        left: 14px;
        top: 50%;
        transform: translateY(-50%);
        color: var(--gray-400);
        font-size: 18px;
        pointer-events: none;
      }
      #search {
        width: 100%;
        padding: 12px 14px 12px 44px;
        border: 2px solid #e0dfd8;
        border-radius: 12px;
        font-size: 14px;
        background: #fff;
        outline: none;
        transition: border-color 0.12s;
        font-family: inherit;
      }
      #search:focus {
        border-color: var(--purple-400);
      }
      #no-results {
        display: none;
        text-align: center;
        padding: 3rem;
        color: var(--gray-400);
        font-size: 15px;
      }
      /* Section headers */
      .discipline-section {
        margin-bottom: 3rem;
      }
      .discipline-header {
        display: flex;
        align-items: center;
        gap: 12px;
        margin-bottom: 1.25rem;
        padding-bottom: 10px;
        border-bottom: 2px solid;
      }
      .discipline-header.ie-header {
        border-color: var(--purple-200);
      }
      .discipline-header.me-header {
        border-color: var(--teal-200);
      }
      .discipline-header.se-header {
        border-color: var(--coral-200);
      }
      .discipline-icon {
        width: 40px;
        height: 40px;
        border-radius: 10px;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 20px;
      }
      .ie-header .discipline-icon {
        background: var(--purple-50);
        color: var(--purple-600);
      }
      .me-header .discipline-icon {
        background: var(--teal-50);
        color: var(--teal-600);
      }
      .se-header .discipline-icon {
        background: var(--coral-50);
        color: var(--coral-600);
      }
      .discipline-title {
        font-size: 18px;
        font-weight: 700;
      }
      .ie-header .discipline-title {
        color: var(--purple-900);
      }
      .me-header .discipline-title {
        color: var(--teal-900);
      }
      .se-header .discipline-title {
        color: var(--coral-900);
      }
      .discipline-count {
        margin-left: auto;
        font-size: 12px;
        font-weight: 600;
        padding: 3px 10px;
        border-radius: 20px;
      }
      .ie-header .discipline-count {
        background: var(--purple-50);
        color: var(--purple-600);
      }
      .me-header .discipline-count {
        background: var(--teal-50);
        color: var(--teal-600);
      }
      .se-header .discipline-count {
        background: var(--coral-50);
        color: var(--coral-600);
      }
      /* Role cards */
      .roles-grid {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
        gap: 1rem;
      }
      .role-card {
        background: #fff;
        border: 1.5px solid #e0dfd8;
        border-radius: 14px;
        padding: 1.25rem;
        transition:
          transform 0.12s ease,
          box-shadow 0.12s ease;
      }
      .role-card:hover {
        transform: translateY(-3px);
        box-shadow: 0 8px 20px rgba(0, 0, 0, 0.07);
      }
      .role-card.ie-card {
        border-left: 4px solid var(--purple-400);
      }
      .role-card.me-card {
        border-left: 4px solid var(--teal-200);
      }
      .role-card.se-card {
        border-left: 4px solid var(--coral-200);
      }
      .role-title {
        font-size: 15px;
        font-weight: 700;
        margin-bottom: 8px;
        line-height: 1.3;
      }
      .ie-card .role-title {
        color: var(--purple-900);
      }
      .me-card .role-title {
        color: var(--teal-900);
      }
      .se-card .role-title {
        color: var(--coral-900);
      }
      .role-desc {
        font-size: 13px;
        color: var(--gray-600);
        line-height: 1.65;
        margin-bottom: 12px;
      }
      .role-tags {
        display: flex;
        flex-wrap: wrap;
        gap: 5px;
      }
      .tag {
        display: inline-flex;
        align-items: center;
        gap: 4px;
        padding: 3px 9px;
        border-radius: 20px;
        font-size: 11px;
        font-weight: 500;
      }
      .tag-eng {
        background: var(--purple-50);
        color: var(--purple-800);
      }
      .tag-ind {
        background: var(--teal-50);
        color: var(--teal-600);
      }
      .tag-prod {
        background: var(--amber-50);
        color: var(--amber-400);
      }
      footer {
        text-align: center;
        padding: 1.5rem;
        font-size: 12px;
        color: var(--gray-400);
        border-top: 1px solid var(--gray-100);
      }
    </style>
  </head>
  <body>
    <nav class="nav-bar">
      <a href="index.html" class="back-link"
        ><i class="ti ti-arrow-left"></i> Home</a
      >
      <span class="nav-count" id="nav-count">37 roles</span>
    </nav>

    <header>
      <div class="header-badge">
        <i class="ti ti-book-open"></i> Career Reference
      </div>
      <h1>📋 Engineering Careers</h1>
      <p>
        Browse all 37 engineering roles — see what each does and how to find it
        in the game
      </p>
    </header>

    <main class="main">
      <div class="search-wrap">
        <i class="ti ti-search"></i>
        <input
          type="text"
          id="search"
          placeholder="Search roles by name or keyword…"
          oninput="filterRoles(this.value)"
        />
      </div>

      <div id="no-results">
        No roles match your search. Try a different keyword.
      </div>

      <div id="ie-section" class="discipline-section">
        <div class="discipline-header ie-header">
          <div class="discipline-icon"><i class="ti ti-chart-arrows"></i></div>
          <div class="discipline-title">Industrial Engineering</div>
          <div class="discipline-count ie-count">18 roles</div>
        </div>
        <div class="roles-grid" id="ie-grid"></div>
      </div>

      <div id="me-section" class="discipline-section">
        <div class="discipline-header me-header">
          <div class="discipline-icon"><i class="ti ti-assembly"></i></div>
          <div class="discipline-title">Manufacturing Engineering</div>
          <div class="discipline-count me-count">10 roles</div>
        </div>
        <div class="roles-grid" id="me-grid"></div>
      </div>

      <div id="se-section" class="discipline-section">
        <div class="discipline-header se-header">
          <div class="discipline-icon"><i class="ti ti-sitemap"></i></div>
          <div class="discipline-title">Systems Engineering</div>
          <div class="discipline-count se-count">9 roles</div>
        </div>
        <div class="roles-grid" id="se-grid"></div>
      </div>
    </main>

    <footer>
      Engineering Career Explorer &nbsp;·&nbsp; Industrial, Manufacturing &amp;
      Systems Engineering
    </footer>

    <script>
      const engLabels = {
        IE: "Industrial Engineering",
        ME: "Manufacturing Engineering",
        SE: "Systems Engineering",
      };
      const indLabels = {
        hospital: "Hospitals",
        factory: "Factories",
        aerospace: "Aerospace",
        logistics: "Logistics",
        bizops: "Business Operations",
        healthcare: "Healthcare",
        supplychain: "Supply Chain",
        technology: "Technology",
        energy: "Energy",
        automotive: "Automotive",
        construction: "Construction",
        warehouse: "Warehouses",
        airport: "Airports",
        food: "Food Production",
      };
      const prodLabels = {
        computers: "Computers",
        floorlayout: "Production Floor Layout",
        quality: "Quality Control",
        costs: "Reducing Costs",
        safety: "Improving Safety",
        productivity: "Increasing Productivity",
        data: "Data Analysis",
        inventory: "Inventory Management",
        process: "Process Improvement",
        medical: "Medical Devices",
        delivery: "Faster Delivery",
        customer: "Better Customer Service",
        robotics: "Robotics / Automation",
        waste: "Waste Reduction",
        scheduling: "Scheduling Workers",
        patientflow: "Improving Patient Flow",
      };

      const roleMap = {
        "IE|hospital|patientflow": {
          role: "Healthcare Systems Engineer",
          desc: "A Healthcare Systems Engineer applies industrial engineering methods to hospitals and clinics — improving patient flow, reducing wait times, and making care delivery safer and more efficient.",
        },
        "IE|factory|costs": {
          role: "Process Improvement Engineer",
          desc: "A Process Improvement Engineer identifies waste and inefficiencies in manufacturing or business operations and redesigns processes to cut costs and boost performance.",
        },
        "ME|factory|floorlayout": {
          role: "Manufacturing Engineer",
          desc: "A Manufacturing Engineer designs and optimizes factory floor layouts, production lines, and assembly systems to maximize output and quality.",
        },
        "ME|food|quality": {
          role: "Quality Engineer",
          desc: "A Quality Engineer ensures products, processes, and systems meet strict standards — reducing defects, minimizing errors, and improving customer satisfaction.",
        },
        "SE|aerospace|computers": {
          role: "Aerospace Systems Engineer",
          desc: "An Aerospace Systems Engineer integrates avionics, software, and hardware subsystems in aircraft or spacecraft to ensure mission success and system reliability.",
        },
        "IE|supplychain|delivery": {
          role: "Supply Chain Engineer",
          desc: "A Supply Chain Engineer designs and optimizes the flow of goods, information, and resources across a supply chain to ensure products arrive faster and at lower cost.",
        },
        "IE|warehouse|inventory": {
          role: "Logistics Engineer",
          desc: "A Logistics Engineer plans and improves warehouse layouts, inventory systems, and distribution networks to move products efficiently and accurately.",
        },
        "ME|factory|robotics": {
          role: "Automation Engineer",
          desc: "An Automation Engineer designs and implements robotic systems, automated machinery, and smart manufacturing solutions that increase speed, precision, and safety.",
        },
        "SE|technology|computers": {
          role: "Technology Systems Analyst",
          desc: "A Technology Systems Analyst designs, integrates, and optimizes complex software and hardware systems to meet the evolving needs of tech-driven organizations.",
        },
        "IE|bizops|data": {
          role: "Operations Analyst",
          desc: "An Operations Analyst uses data analysis, modeling, and optimization techniques to improve business processes, reduce costs, and support strategic decision-making.",
        },
        "IE|factory|process": {
          role: "Process Engineer",
          desc: "A Process Engineer analyzes and redesigns manufacturing or business workflows to eliminate bottlenecks, reduce variation, and improve overall throughput and quality.",
        },
        "IE|factory|safety": {
          role: "Safety & Industrial Engineer",
          desc: "A Safety & Industrial Engineer combines ergonomics, process design, and risk analysis to build safer workplaces while maintaining productivity.",
        },
        "IE|factory|productivity": {
          role: "Industrial Engineer",
          desc: "An Industrial Engineer optimizes systems of people, machines, and materials to maximize productivity and eliminate waste across manufacturing and service environments.",
        },
        "ME|automotive|robotics": {
          role: "Automotive Automation Engineer",
          desc: "An Automotive Automation Engineer develops robotic assembly lines and smart manufacturing systems for vehicle production, improving speed, precision, and safety.",
        },
        "ME|automotive|quality": {
          role: "Automotive Quality Engineer",
          desc: "An Automotive Quality Engineer ensures vehicles and components meet rigorous safety and performance standards throughout design, production, and delivery.",
        },
        "SE|energy|computers": {
          role: "Energy Systems Engineer",
          desc: "An Energy Systems Engineer designs and manages integrated power systems — from smart grids to renewable energy infrastructure — ensuring reliable and efficient energy delivery.",
        },
        "IE|hospital|costs": {
          role: "Healthcare Operations Engineer",
          desc: "A Healthcare Operations Engineer applies lean and industrial engineering principles to reduce costs in hospitals while maintaining high-quality patient care.",
        },
        "IE|logistics|delivery": {
          role: "Logistics & Distribution Engineer",
          desc: "A Logistics & Distribution Engineer designs transportation networks and fulfillment systems to ensure goods reach customers faster and at lower operational cost.",
        },
        "SE|airport|scheduling": {
          role: "Airport Systems Engineer",
          desc: "An Airport Systems Engineer plans and optimizes complex airport operations — from gate scheduling and baggage systems to security flow and control systems integration.",
        },
        "IE|factory|scheduling": {
          role: "Production Planning Engineer",
          desc: "A Production Planning Engineer schedules workers, machines, and materials on the factory floor to meet demand, reduce downtime, and optimize shift efficiency.",
        },
        "ME|food|robotics": {
          role: "Food Automation Engineer",
          desc: "A Food Automation Engineer designs automated processing and packaging systems in food manufacturing to improve throughput, hygiene, and consistency.",
        },
        "SE|technology|data": {
          role: "Data Systems Engineer",
          desc: "A Data Systems Engineer architects and integrates large-scale data platforms, pipelines, and analytics infrastructure to support intelligent decision-making.",
        },
        "IE|healthcare|medical": {
          role: "Biomedical & Industrial Engineer",
          desc: "This engineer combines industrial engineering methods with healthcare expertise to optimize the deployment, maintenance, and workflow around medical devices and clinical equipment.",
        },
        "ME|factory|waste": {
          role: "Lean Manufacturing Engineer",
          desc: "A Lean Manufacturing Engineer applies lean and Six Sigma principles to eliminate waste — overproduction, idle time, defects — and create more value with fewer resources.",
        },
        "IE|bizops|customer": {
          role: "Customer Experience & Operations Engineer",
          desc: "This engineer uses operations research and process improvement methods to redesign service systems, reduce friction, and elevate the customer experience.",
        },
        "IE|construction|safety": {
          role: "Construction Safety Engineer",
          desc: "This engineer designs safe construction workflows, site layouts, and worker scheduling systems to reduce accidents and keep projects on time and on budget.",
        },
        "SE|aerospace|safety": {
          role: "Systems Safety Engineer",
          desc: "A Systems Safety Engineer identifies and mitigates risks in complex aerospace and defense systems, ensuring every subsystem meets strict reliability and safety requirements.",
        },
        "ME|healthcare|medical": {
          role: "Medical Device Manufacturing Engineer",
          desc: "A Medical Device Manufacturing Engineer designs and validates production processes for medical equipment, ensuring devices are safe, effective, and compliant with regulatory standards.",
        },
        "SE|energy|data": {
          role: "Energy Data Systems Engineer",
          desc: "An Energy Data Systems Engineer builds integrated systems that collect, process, and visualize power grid and energy consumption data to drive smarter energy management decisions.",
        },
        "IE|food|quality": {
          role: "Food Quality & Industrial Engineer",
          desc: "This engineer applies quality control methods and lean principles to food production lines, ensuring consistent product standards and regulatory compliance.",
        },
        "IE|airport|scheduling": {
          role: "Airport Operations & Industrial Engineer",
          desc: "This engineer uses industrial engineering tools to optimize staffing, gate assignments, and terminal flow at airports, reducing delays and improving passenger experience.",
        },
        "SE|construction|safety": {
          role: "Construction Systems Engineer",
          desc: "A Construction Systems Engineer integrates project planning, safety systems, and structural requirements to ensure complex construction projects are delivered safely and on schedule.",
        },
        "ME|energy|robotics": {
          role: "Energy Automation Engineer",
          desc: "An Energy Automation Engineer designs automated systems for power plants and renewable energy installations, improving efficiency, safety, and uptime.",
        },
        "IE|warehouse|scheduling": {
          role: "Warehouse Operations Engineer",
          desc: "A Warehouse Operations Engineer optimizes worker schedules, picking routes, and storage layouts to maximize throughput and minimize operational cost.",
        },
        "SE|technology|process": {
          role: "IT Process Systems Engineer",
          desc: "An IT Process Systems Engineer designs and integrates enterprise software workflows, ensuring that IT systems and business processes are aligned, scalable, and efficient.",
        },
        "IE|supplychain|inventory": {
          role: "Supply Chain & Inventory Engineer",
          desc: "This engineer develops inventory management strategies and supply chain models that balance stock levels, reduce holding costs, and prevent stockouts across distribution networks.",
        },
        "ME|construction|floorlayout": {
          role: "Construction Manufacturing Engineer",
          desc: "A Construction Manufacturing Engineer designs prefabrication layouts and modular construction workflows, bringing manufacturing efficiency to building and infrastructure projects.",
        },
      };

      const allRoles = Object.entries(roleMap).map(([key, val]) => {
        const [eng, ind, prod] = key.split("|");
        return { key, eng, ind, prod, role: val.role, desc: val.desc };
      });

      function renderRole(r) {
        const typeClass = r.eng.toLowerCase();
        return `
    <div class="role-card ${typeClass}-card" data-search="${r.role.toLowerCase()} ${r.desc.toLowerCase()} ${engLabels[r.eng].toLowerCase()} ${indLabels[r.ind].toLowerCase()} ${prodLabels[r.prod].toLowerCase()}">
      <div class="role-title">${r.role}</div>
      <div class="role-desc">${r.desc}</div>
      <div class="role-tags">
        <span class="tag tag-eng">${engLabels[r.eng]}</span>
        <span class="tag tag-ind">${indLabels[r.ind]}</span>
        <span class="tag tag-prod">${prodLabels[r.prod]}</span>
      </div>
    </div>`;
      }

      function renderAll() {
        document.getElementById("ie-grid").innerHTML = allRoles
          .filter((r) => r.eng === "IE")
          .map(renderRole)
          .join("");
        document.getElementById("me-grid").innerHTML = allRoles
          .filter((r) => r.eng === "ME")
          .map(renderRole)
          .join("");
        document.getElementById("se-grid").innerHTML = allRoles
          .filter((r) => r.eng === "SE")
          .map(renderRole)
          .join("");
      }

      function filterRoles(query) {
        const q = query.trim().toLowerCase();
        const cards = document.querySelectorAll(".role-card");
        let visible = 0;
        cards.forEach((card) => {
          const matches = !q || card.dataset.search.includes(q);
          card.style.display = matches ? "" : "none";
          if (matches) visible++;
        });
        document.getElementById("nav-count").textContent =
          visible + " role" + (visible !== 1 ? "s" : "");
        document.getElementById("no-results").style.display =
          visible === 0 ? "block" : "none";
        ["ie", "me", "se"].forEach((t) => {
          const section = document.getElementById(`${t}-section`);
          const gridCards = section.querySelectorAll(".role-card");
          const anyVisible = Array.from(gridCards).some(
            (c) => c.style.display !== "none",
          );
          section.style.display = anyVisible ? "" : "none";
        });
      }

      renderAll();
    </script>
  </body>
</html>
```

- [ ] **Step 2: Verify in browser**

Open `careers.html`. Confirm:

- Three sections render: Industrial Engineering (18 cards), Manufacturing Engineering (10 cards), Systems Engineering (9 cards)
- Each role card shows title, description, and three colored tag chips
- Searching "hospital" shows only hospital-related roles and hides empty sections
- Searching "robot" shows Automation Engineer, Automotive Automation Engineer, Food Automation Engineer, Energy Automation Engineer
- Nav count updates to show number of matching roles
- "No roles match" message shows when search yields zero results
- Clicking "Home" in the nav returns to `index.html`
