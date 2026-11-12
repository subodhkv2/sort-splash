# Sort Splash: Colour Tube Puzzle

A single-file HTML5 puzzle game built for YouTube Playables.

- **Developer account for Playables (user's explicit choice):** `subodhsinghlive@gmail.com`
  - Do NOT use `subodhkumarvermakv@gmail.com` (previous Playables choice, switched 12 Nov 2026).
  - Do NOT use `hello@getcerttracker.com`.
  - No sign-in was performed while building this game. Nothing was submitted anywhere.
- **Audience:** Ages **13+ only**, general audience, English (US tier-1: US / UK / Canada / Australia). **NOT made for kids.** No child characters, no kids branding.
- **Version:** **v1** (2026-11-12).
- **File:** `index.html` — **63,188 bytes (~61.7 KB)**, self-contained. No external libraries, images, fonts, or audio files. The only external URL in the file is the official Playables SDK script tag.
- **Sister game:** Mix & Spell v2 (`../mix-and-spell/`, 60,940 bytes) — same design system, same SDK pattern, different mechanic (colour-mix + spelling vs tube sorting). The two games do not overlap.

## How to play

1. **Pick a tube:** Tap a tube to select its top colour. Tap it again to deselect.
2. **Pour:** Tap another tube to pour the top run of that colour into it. Pour rules, enforced exactly:
   - Only the top contiguous same-colour run moves (up to the free space).
   - The destination must have space, and its top colour must match the poured colour — or the destination must be empty.
   - Pouring into a full or mismatched tube is rejected with a shake + sound and no state change.
3. **Win a level:** Every non-empty tube holds a single colour. A tube that is all one colour and full is "sorted" — the progress bar counts sorted tubes.
4. **Locked tubes (Level 8+):** A locked tube (🔒) cannot be used until you complete a pour elsewhere — each successful pour unlocks one locked tube.
5. **Move limit (Level 12+):** Later levels give you a move budget (solution length + 8). Run out and the level fails — continue for +5 moves via a rewarded ad, or retry fresh. Getting stuck with no legal pours left also ends the level the same way (Undo can back you out).
6. **Score:** 100 points per level + 10 per unused move (limited levels) + up to the par-time bonus + 20 per streak level. Coins: +1 per pour, +5 bonus on your first pour, +10 per level win.
7. **Undo (5 coins):** Restores the tubes, locks and move count from before your last pour. **Hint (10 coins):** The built-in solver suggests one correct pour and highlights both tubes. Both refuse with a message when you cannot afford them — coins never go negative.
8. **Daily Challenge:** One level a day, seeded by the date (the same level for everyone that day), +50 coins on completion. Daily streak counts consecutive days played.
9. **Endless mode:** After Level 50 the generator keeps producing new levels. Difficulty ramps — see below.
10. **Modes:** **Classic** (levels, above) and **60-Second Blitz** — mini 3-colour puzzles, sort as many as you can in 60 seconds, combo multiplier (50 × combo per sort), instant restart. A rejected pour in Blitz resets your combo.
11. **Progression:** achievements (First Pour, 10 Levels, Blitz 5, Daily 3-Day, Flawless Win), a daily mission on the menu ("Win 2 levels today", +30 coins), a Day 1–7 daily reward calendar (10/20/30/40/50/75/100 coins), unlockable colour themes (Sunset 100, Ocean 200, Forest 300 coins — no purchases), and your personal best on the menu.
12. **Onboarding:** a big "▶ TAP TO PLAY" button, a 3-step animated mini-tutorial (Pick a tube → Pour it → Sort them all, under 20 seconds, auto-advances every 6 s, skippable), and tutorial-easy Levels 1–3 (3 colours, 5 tubes, no locks, no move limit, practice hints). First coins land in the first seconds of play (first pour pays +5 bonus + 1 coin).

## Difficulty ramp

| Levels | Colours | Tubes | Empty | Locked | Move limit | Un-pours* |
|---|---|---|---|---|---|---|
| 1–3 | 3 | 5 | 2 | 0 | none | 3–5 |
| 4–7 | 4 | 6 | 2 | 0 | none | 5–7 |
| 8–11 | 5 | 7 | 2 | 1 | none | 7–9 |
| 12–19 | 5 | 6 | 1 | 1 | solution + 8 | 9–11 |
| 20–50+ | 6 | 7 | 1 | 2 | solution + 8 | 12–14 |

\* Levels are procedurally generated and **guaranteed solvable**: the generator starts from the solved state and applies random valid reverse-pours, then verifies the reverse sequence replays to solved — and the test suite's independent BFS solver proves every generated level (levels 1–200 tested) is solvable. Par time for the score bonus: 90 s (L1–3) → 120 s → 150 s → 180 s (L20+).

Note on the spec: the brief said "L1–3 tutorial-easy (3 colours, 4 tubes, 2 empty)". 3 colours need 3 tubes, plus 2 empty tubes = 5 tubes, so 4 total is arithmetically impossible; the game uses 5 tubes (3 colour + 2 empty) and this is the documented interpretation.

## Design System (same as Mix & Spell v2, for brand consistency)

- **Base theme — Royal Indigo:** background gradient `#0F172A → #1E1B4B → #312E81`; glassmorphism cards (rgba white 8%, blur 12px, 1px light border, 20px radius, soft shadow); text `#F8FAFC`, muted `#CBD5E1` (WCAG AA — never light grey on dark).
- **Primary:** Indigo `#6366F1` → Violet `#8B5CF6` gradient on buttons/accents. **Gold `#F59E0B / #FBBF24` only** for coins, rewards, streaks, achievements. Success `#10B981`, Danger `#EF4444`, Warning `#F59E0B`.
- **Typography:** system font stack only — `-apple-system, "Segoe UI", Roboto, Helvetica, Arial` (no external fonts, Playables rule). Title 800 / 32px / −0.5px letter-spacing, gradient text. Section headings 700 / 20px. Body 500 / 16px. Buttons 700 / 16px, normal case.
- **Spacing:** 8pt grid (8/16/24px); cards padding 20px; buttons min-height 52px; tap targets ≥48px (tubes 64px wide, 132px tall); radius 16px cards / 999px pill buttons / 12px chips.
- **Micro-interactions (CSS only):** button press scale 0.97, hover lift, selected tube lifts + gold glow, wrong pour = gentle shake, correct pour = droplet animation + "+1 coin" popup, level complete = confetti burst (CSS-drawn), streak = flame glow, level-up banner, timer bar green → amber → red, level progress bar (sorted tubes / total colours).
- **Game colours — colour-blind safe:** every colour has a name label AND a distinct shape glyph (never colour alone), shown on every layer and under every tube. Red `#EF4444` ●, Blue `#3B82F6` ■, Yellow `#FACC15` ▲ (dark `#1F2937` ink — the only light colour), Green `#22C55E` ◆, Purple `#A855F7` ★, Orange `#F97316` ⬢, Pink `#EC4899` ♥, Teal `#14B8A6` ✚.
- **Unlockable themes (coins only, NO purchases)** — each changes the FULL palette, not just the background: (1) Default Royal Indigo, (2) Sunset — bg `#1C0A2E → #7C2D12`, primary `#F97316 → #EC4899`, (3) Ocean — bg `#082F49 → #0E7490`, primary `#06B6D4 → #3B82F6`, (4) Forest — bg `#052E16 → #14532D`, primary `#22C55E → #14B8A6`. Theme switch applies instantly, saves via the SDK, and persists across reload.
- **One style on every screen:** menu, play, blitz, pause, level-complete, level-failed — same header, same buttons, same spacing. Menu shows the game name in big gradient text with best score / coins / streak chips.

## Controls

- **Touch / mouse:** tap tubes and buttons.
- **Keyboard:** `1–7` pick a tube · `U` undo · `H` hint · `Esc` pause.
- **Pause:** Pause button, `Esc`, switching tabs, or the platform's pause signal all pause the game and stop the timers.
- **Sound:** WebAudio oscillator beeps only; a Sound On/Off button, plus the platform mute signal, silences them.

## Monetization hooks (stubbed — no real ads in this file)

- **Interstitial:** offered only on the level-complete screen, after every 3 levels, and only if at least 90 seconds have passed since the last one. Never mid-gameplay.
- **Rewarded:** on the level-failed screen, "Continue: +5 moves (ad)" asks the platform for a rewarded ad. The moves are granted ONLY if the ad actually played (or, in a normal browser with no platform, via the local stub so the button stays testable). If the ad is declined or fails, no moves are given and the game stays on the failed screen with an explanation.
- **Pre-roll:** YouTube's own — nothing for the game to do.
- No in-app purchases, no login, no external links, no sharing prompts, no quit button.

## How to test locally

1. Open `index.html` in any modern browser (Chrome, Firefox, Safari, Edge) — desktop or phone. No server needed; double-click works.
2. Or serve it: `python3 -m http.server 8000` from this folder, then open `http://localhost:8000/index.html`.
3. Play at least Levels 1–12 (see `TEST-REPORT.md` for the automated playthrough).
4. Resize the window / rotate the phone: the layout is responsive for portrait, landscape, 1:1, 16:9 and 9:16.
5. Check the browser console: there should be no errors and no network requests except the SDK script (which is a no-op locally if it fails to load — the built-in stub takes over).
6. Run the automated suites: `node tests/unit.js`, `node tests/playthrough.js`, `node tests/full.js`, `node tests/new.js` — all must pass with 0 failures (492 checks total).

## What is NOT done yet (needs the user, not this build)

- **Playables interest form + Developer Portal approval.** Access is invitation-only and early-access; this build does not submit anything and cannot bypass that gate. (The Mix & Spell interest form was already submitted 12 Nov 2026 from the same account; this game would need its own portal listing.)
- **Real certification** in the Playables Developer Portal (test suite on desktop web, mobile web, Android, iOS) and **real ad testing** — both need portal access.
- **Thumbnails / store metadata** for the portal listing — see `COMPLIANCE-CHECKLIST.md` for what to prepare.

## Files in this folder

| File | What it is |
|---|---|
| `index.html` | The complete game |
| `README.md` | This file |
| `COMPLIANCE-CHECKLIST.md` | Every Playables rule, with PASS + evidence |
| `TEST-REPORT.md` | Self-review: automated tests + playthrough + bugs found/fixed |
| `tests/unit.js` | Pure-logic tests: pour rules, solver, 200-level generator proof, save, compliance greps, ID guard |
| `tests/playthrough.js` | Stubbed-DOM playthrough: levels 1–8 + Blitz + daily, undo/hint/locks |
| `tests/full.js` | Full-scenario suite: SDK mock, timer, ads, save/load, keyboard, stress |
| `tests/new.js` | New-feature suite: tutorial, Blitz combo, achievements/missions/themes, daily, juice, design |
| `tests/screenshots/` | Headless-Chrome screenshots (menu / play / Blitz, 4 themes, 4 sizes) |
