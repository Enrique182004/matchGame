# Engineering Career Game — Design Spec

Date: 2026-06-09

## Overview

Remake the existing single-page `engineering_career_explorer.html` into a polished, game-like multi-page web experience deployable on GitHub Pages. No build step; all files are self-contained HTML with inline CSS and JS.

## Audience

General public — accessible, fun, no assumed engineering knowledge.

## File Structure

```
index.html      Landing page with two navigation options
game.html       The interactive card-matching game
careers.html    Informative reference of all 37 engineering roles
```

## index.html — Landing Page

- Full-viewport hero with game title and subtitle
- Two large clickable hero cards:
  - "Play the Game" → game.html
  - "Explore Careers" → careers.html
- Consistent header (purple/teal gradient) and footer (UTEP / Industrial Engineering credit)
- Same CSS variable color system as existing file

## game.html — The Game

### Top bar

- Back link to index.html
- Score badge: "X matches found"
- Streak badge: "🔥 X streak"
- Personal best timer: "Best: Xs"

### Mechanic (preserved from original)

- Step 1: Pick Engineering Type (IE / ME / SE)
- Step 2: Pick Industry (14 options)
- Step 3: Pick Product/Problem (16 options)
- Auto-evaluate on all 3 selected

### Match result

- Confetti burst animation
- Role card slides in with title + description + "Found!" badge
- Score +1, streak +1
- Timer records time-to-match

### No match result

- Cards briefly shake with red flash
- "Try a different combo!" hint message
- Streak resets to 0

### Try Again

- Resets card selections and result area only
- Score and streak persist for the session

## careers.html — Career Explorer

- Search bar (live filter by role name or keyword)
- 37 roles grouped into 3 sections: Industrial Engineering, Manufacturing Engineering, Systems Engineering
- Each role card shows:
  - Role title
  - Description paragraph
  - Three tag chips: engineering type + industry + problem (doubles as game hint)

## Visual Design

- Color palette: purple (#534AB7 primary), teal (#0F6E56), coral (#993C1D), warm gray background
- Card animations: staggered fade-up on load, pop/glow on selection
- Confetti: CSS-only or lightweight JS on match
- Responsive: mobile-friendly flex layouts

## Data

All role data from existing `roleMap` (37 entries) is preserved exactly. No data is added or removed.
