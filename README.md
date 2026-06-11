# 🐍 Snake

A classic Snake game built with a single HTML file — no frameworks, no dependencies, no build step.

---

## How It Was Built

### Architecture

The entire project lives in one self-contained `index.html` file with inline CSS and JavaScript. The game renders on a `<canvas>` element and all logic runs in a simple interval-based loop.

### Rendering

Each tick the canvas is fully cleared and redrawn using the **Canvas 2D API**. The grid is 20×20 cells, each cell sized at `canvas.width / GRID` (24 px). Rounded rectangles are drawn manually via `arcTo` since `ctx.roundRect` has limited browser support.

### Game Loop

Unlike RAF-based games, Snake uses `setInterval` for its loop — appropriate here because the game is turn-based rather than continuous:

```
setInterval(tick, speed)
  └── move snake head by one cell
  └── check wall / self collision
  └── check food collision
  └── draw()
```

Speed starts at 120 ms per tick and decreases by 3 ms on each food pickup, down to a floor of 60 ms.

### Snake Movement

The snake is stored as an array of `{x, y}` grid coordinates. Each tick a new head is prepended (`unshift`). If food is eaten the tail stays (snake grows); otherwise it is removed (`pop`). A `nextDir` buffer prevents reversing direction mid-tick if keys are pressed rapidly.

### Collision Detection

- **Wall:** head coordinates go out of `[0, GRID)` range.
- **Self:** `Array.some()` checks if the new head matches any existing segment.

### Food Placement

A random cell is picked in a `while` loop, rejecting any position already occupied by the snake. Guaranteed to find a free cell as long as the board isn't full.

### Score & Best Score

Score increments by 10 per food pickup. Best score persists across sessions via `localStorage`.

### Overlay

Game Over state is a fixed `<div>` toggled with a `.show` CSS class. Pause and restart are handled with `P` and `R` keys respectively.

---

## How to Run

No installation or server required.

1. Download the file:
   ```
   index.html
   ```

2. Open `index.html` in any modern browser (Chrome, Firefox, Edge, Safari).

3. Press any arrow key or WASD to start moving.

That's it.

---

## Controls

| Key | Action |
|---|---|
| Arrow keys / WASD | Move |
| P | Pause / Resume |
| R | Restart |
