## Context

The mini-game lives entirely in `html/play.html` as a self-contained Phaser 4 scene loaded from a CDN. The current setup:

- Phaser `Scale` config uses `mode: Phaser.Scale.RESIZE` with `width/height: '100%'`. The canvas always matches the window, but the camera viewport grows with it, so the world width visible on screen grows on large monitors — which is what makes the player and enemies look tiny on desktop.
- Input is keyboard-only via `this.input.keyboard.createCursorKeys()` and a few `addKeys` mappings (A/D/W/Space). There is no pointer/touch handling.
- The page is built with `<meta name="viewport" content="width=device-width, initial-scale=1.0">`, full-viewport `<div id="game">`, and no other HTML chrome besides the "Home" back-link.

Constraints from CLAUDE.md: pure static site, no build step, must work on GitHub Pages and ideally via `file://`. Target audience is non-technical adults — controls have to be obvious.

## Goals / Non-Goals

**Goals:**
- Playable on touch devices with on-screen controls that mirror the keyboard.
- The same game looks well-scaled on desktop monitors (player and enemies occupy a comparable share of the screen as on mobile).
- Keep the implementation simple: a single HTML file edit, no new dependencies, no build step.

**Non-Goals:**
- Portrait orientation support — landscape only.
- Reworking the gameplay, levels, or art.
- Gamepad support.
- A configurable / remappable control scheme.

## Decisions

### Switch Phaser scale mode from `RESIZE` to `FIT` with a fixed base resolution
- Pick a base resolution (e.g. `1280×720`, 16:9 landscape) and let Phaser letterbox to fit the viewport.
- Why: with `FIT`, sprites, text, and the camera viewport all scale together, so the player on a 1080p monitor looks proportionally the same as on a phone. With `RESIZE`, the camera viewport grows with the window and exposes more world, making everything look small.
- Alternative considered: keep `RESIZE` but multiply camera zoom by viewport area. Rejected — fragile, has to fight Phaser's own resize logic, and breaks the world-width assumption (`WORLD_W = 4000`).
- Trade-off: black letterbox bars on aspect ratios that don't match 16:9. Acceptable for a small edutainment game; the page background is already black.

### Detect touch capability once, render an HTML overlay on top of the canvas
- Use a feature check (`'ontouchstart' in window || navigator.maxTouchPoints > 0`) on page load.
- Render the overlay in plain HTML/CSS positioned over `#game` with `pointer-events` allowed on the buttons only. Keep it out of Phaser to avoid coupling DOM input to scene lifecycle and to keep CSS scaling simple.
- Why HTML/CSS instead of drawing buttons inside Phaser: simpler to style with media queries, simpler to position via `vh`/`vw`, no extra Phaser scene logic, accessible to standard touch event handling.

### Map touch events to the same input state the scene already reads
- Introduce a small `touchInput = { left: false, right: false, jumpPressed: false, shootPressed: false }` flag object on `window` (or on the scene) updated by `pointerdown` / `pointerup` (and `pointercancel`, `pointerleave`) on each overlay button.
- In the scene's `update()`, OR the touch flags with the existing keyboard checks (`left || touchInput.left`, etc.). For "just pressed" actions like jump and shoot, set a one-shot flag on `pointerdown` and consume it in the next `update()`.
- Why: minimally invasive — the existing input branch in `update()` already ORs keyboard and cursor inputs; we add one more source.

### Overlay layout
- Right side: a circular D-pad or a pair of side-by-side arrow buttons for left/right movement, anchored bottom-right with a margin.
- Left side: two larger round buttons for jump and shoot, anchored bottom-left.
- Sizes use `min(14vh, 14vw)` so buttons scale to the smaller axis and stay reachable for thumbs in landscape.
- Buttons are semi-transparent (`rgba(255,255,255,0.18)`) with a thicker border on press; they sit above the canvas via `z-index` but below the back-link.

### Show overlay only when needed
- The overlay is rendered into the DOM only after the touch-capability check passes (set a `body.touch` class and gate `.touch-controls` visibility with CSS). On desktop the elements are not interactive and not painted.

## Risks / Trade-offs

- **Letterboxing at extreme aspect ratios** → mitigation: black bars match the page background, so they're not visually jarring.
- **Hybrid devices (touchscreen laptops)** → both keyboard and touch overlay become available; that's fine, the inputs are OR'd. Acceptable extra UI rather than mis-detecting and breaking touch users.
- **`pointerleave` while a button is held** (finger slides off) → mitigation: also listen to `pointercancel`, and reset all touch flags on `visibilitychange` to hidden, so the player doesn't get stuck moving.
- **iOS Safari double-tap zoom / long-press menu on buttons** → mitigation: `touch-action: none` on the overlay, `user-select: none`, and `preventDefault()` on the relevant pointer events.
- **Base resolution choice (1280×720)** → on very tall narrow windows letterboxing grows; acceptable since we explicitly assume landscape.

## Migration Plan

Single-PR change to `html/play.html` (and possibly `html/css/style.css` for the overlay styles). No data migration. Rollback is `git revert`.

## Open Questions

- None blocking. (Exact base resolution and exact button artwork are implementation details — decide during apply.)
