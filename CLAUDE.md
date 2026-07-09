# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

A clone of the arcade classic **Asteroids** built with pure HTML5 Canvas — no dependencies, no bundler, no build step. All game logic lives in a single file: `game.js`.

## Running

Open `index.html` directly in a browser, or serve it locally:

```bash
npx serve .
```

Then visit `http://localhost:3000`. There is no build, lint, or test tooling in this repo — verify changes by loading the page and playing.

## Architecture

Everything is in `game.js`, structured as a classic real-time game loop over a small set of entity classes:

- **Entities**: `Bullet`, `Asteroid`, `Ship`, `Particle` — each has `update(dt)` and `draw()`. Sizes/speeds/points for asteroids are driven by parallel arrays indexed by size (`RADII`, `SPEEDS`, `POINTS`, sizes 1–3, small→large).
- **Global mutable state**: `ship`, `bullets`, `asteroids`, `particles`, `score`, `lives`, `level`, `state` (`'playing' | 'dead' | 'gameover'`) are module-level `let` bindings reassigned by `initGame()` / `nextLevel()`, not encapsulated in a class or object.
- **Game loop**: `requestAnimationFrame(loop)` computes `dt` (clamped to 0.05s) and calls `update(dt)` then `draw()` every frame. `update` branches on `state` first (gameover/dead have their own short-circuited logic) before running normal gameplay update (input, movement, collisions, level transitions).
- **Input**: raw keyboard state in `keys` (held) and `justPressed` (edge-triggered, consumed via `pressed(code)`) populated by `keydown`/`keyup` listeners at the top of the file.
- **Collisions**: brute-force O(n²) distance checks (`dist`) between bullets/asteroids and ship/asteroids each frame — no spatial partitioning, appropriate given the small entity counts.
- **World wrap**: all positions wrap toroidally via `wrap(v, max)` — the canvas is `W=800` by `H=600`.
- **Asteroid splitting**: `Asteroid.split()` produces two smaller asteroids (size − 1) on destruction; size 1 asteroids don't split.

When adding new entities or mechanics, follow the existing pattern: a class with `update(dt)`/`draw()`, pushed into one of the global arrays, filtered for `.dead` each frame in `update()`.

## Testing:

Vitests = usar la skill y la información: de @


