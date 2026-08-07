# AGENTS.md

Single-file Asteroids clone in vanilla JS + HTML5 Canvas. No build, no deps, no tests, no npm scripts.

## Run / verify

- Open `index.html` directly, or `npx serve .` then `http://localhost:3000`.
- There is no test suite, linter, typecheck, or formatter configured. Verify changes by loading `index.html` in a browser and playing through a level.

## Architecture

- All game logic lives in `game.js`, loaded by `index.html` via a plain `<script src>`. No modules, no bundler.
- Canvas is fixed at 800×600 (`W`/`H`); world is toroidal via `wrap()`.
- Entities are plain ES6 classes (`Bullet`, `Asteroid`, `Ship`, `Particle`) sharing `x/y/radius/dead` and `update(dt)/draw()` conventions.
- Global state (`ship, bullets, asteroids, particles, score, lives, level, state`) is module-scope; `state` cycles `'playing' | 'dead' | 'gameover'`.
- Main loop is `loop(ts)` with a dt clamp at 0.05s; one `requestAnimationFrame` chain started at the bottom of `game.js`.
- Input uses a global `keys` map plus a read-once `justPressed` map consumed via `pressed(code)` for edge-triggered actions (shoot, restart).
- Asteroid sizes: 3=large, 2=medium, 1=small. `RADII`/`SPEEDS`/`POINTS` arrays are 1-indexed. Large→50/med destroys split into two of size-1.

## Conventions

- Language: code comments and HUD strings are in Spanish; keep that consistent.
- Style: `'use strict'`, 2-space indent, const-first, arrow helpers at top (`wrap`, `dist`, `rand`, `randInt`). No semicolons except the leading directive.
- Drawing uses raw `ctx` calls with save/restore; colors hardcoded (`#fff` strokes, black fill clear).