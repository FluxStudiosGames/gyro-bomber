# Gyro Bomber — build 2

A 2D side-view arcade bomber in one self-contained file (`index.html`, plain canvas + JS). It makes no network requests and uses no external art or audio files. Open `index.html` in a browser, or serve the folder (`python3 -m http.server`).

## Controls
| Action | Keyboard | Touch (phones) |
|---|---|---|
| Fly up/down/left/right | Arrow keys or W/A/S/D | On-screen D-pad (bottom-left corner, compact) |
| Drop bomb | Space (or Enter) | BOMB button (bottom-right corner) |
| Mute | Top-right ♪ button | Same (persists in `localStorage` `gb_mute`) |
| Fullscreen | Top-right ⛶ (hidden if unsupported or on iPhone Safari) | Same |
| Start / restart | Any key | Tap |

On portrait phones a "Rotate your phone" overlay appears (landscape only). Multi-touch is supported (e.g. hold D-pad + BOMB).

## Rules
- The world scrolls right over procedural hilly terrain. The gyrocopter can move freely, but only in the left 70% of the screen.
- Ground targets (points × current level):
  - **Tank** (100): drives slowly and fires aimed shots.
  - **Bunker** (150): takes 2 hits and fires inaccurate shots.
  - **AA flak gun** (200): fires fast aimed shots.
  - **Fuel depot** (50): harmless. Destroying it releases a floating **F+B crate**. Fly into the crate to refill fuel to full and get +8 bombs (max 25) and +50 points.
- **Bombs** are limited (you start with 15) and have a 0.35 s cooldown. Bombs fall with gravity and keep some forward speed — drop early.
- **Fuel** drains all the time. At 0 fuel, the gyro slowly sinks. If you run out of bombs and no depot is ahead, an airdropped crate appears, so a run can't get stuck.
- **Lives:** 3. You lose one when an enemy shot hits you or you touch the ground. After that you respawn with 2 s of invulnerability (flashing) and at least 50% fuel.
- **Difficulty:** gentle for the first ~40 s (fewer/slower enemies, almost no AA), then ramps. Level goes up every 2500 px of distance.
- First play shows a short tip: "Drop bombs early: they fly forward!"
- **Game over** at 0 lives shows your score, level, and best score (saved in `localStorage` key `gb_best`). Press any key or tap to restart.

## Code structure (index.html, one `<script>`)
- `CFG`: all tuning constants (lives, fuel, bombs, speeds, level distance, gravity, `graceSec`).
- **Input**: `keys{}` for the keyboard, `touch{}` for the on-screen buttons (pointer + touch, multi-touch safe), and `inp.*()` helpers.
- **Audio**: WebAudio synth with mute via `gb_mute`.
- **Terrain**: `groundY(worldX)` is a sum of 3 sines with a random seed. `camX` is the world scroll offset.
- **State machine**: `state` is one of `title`, `play`, or `over`. `reset()` starts a new run. `grace()` eases early difficulty.
- `window.__gb` is a tiny test hook (`state`, `score`, `playTime`, `lives`, `kill()`) used only by automated tests.

`screenshots/build-2/` holds test captures from build 2.
