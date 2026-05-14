# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the project

Each `.html` file is a standalone browser game — no build step, no dependencies.

```
python -m http.server 8080
```

Then open `http://localhost:8080/<filename>.html` in a browser.

## Architecture

- Every game is a **single self-contained HTML file** (inline CSS + inline JS). No frameworks, no external assets.
- Rendering is done on a `<canvas>` element with `requestAnimationFrame` game loops.
- Sprites and visuals are **procedurally generated** using offscreen canvases and pixel-block arrays (no image files).
- Game state is managed via plain objects and a state machine enum (`MENU` → `PLAYING` → etc.), not classes.

### tactical-shooter.html
- 960×640 logical canvas, 2400×1800 world with smooth-follow camera
- State machine: `MENU`, `PLAYING`, `LEVEL_TRANSITION`, `GAME_OVER`
- Entity system: `createPlayer()`, `createEnemy()`, `createBullet()`, `spawnParticle()`
- Collision: brute-force circle-circle (entity count stays under 50)
- Particle pool: 400 pre-allocated objects with `active` flag to avoid GC pressure
- Level configs in `LEVELS[]` array — adjust difficulty there

### tictactoe.html
- DOM-based rendering (not canvas), simpler architecture
- Plain win-check logic with a static `wins` array of index triplets

## Git workflow

After every meaningful change (feature added, bug fixed, file created or significantly refactored):

1. Stage and commit with a clean, descriptive message:
   ```
   git add <files>
   git commit -m "concise description of what changed and why"
   ```
2. Push immediately so nothing is lost:
   ```
   git push
   ```

Remote: `https://github.com/automan2006/claude-code-test`

This ensures every working state is saved and recoverable. Do not batch unrelated changes into one commit.
