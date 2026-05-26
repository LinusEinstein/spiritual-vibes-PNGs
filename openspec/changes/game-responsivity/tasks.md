## 1. Scaling

- [x] 1.1 Pick a base resolution (1280×720, 16:9) and add it as constants in `play.html`.
- [x] 1.2 Change the Phaser `Scale` config in `play.html` from `mode: RESIZE` + `width/height: '100%'` to `mode: Phaser.Scale.FIT`, `autoCenter: Phaser.Scale.CENTER_BOTH`, and the fixed base width/height.
- [x] 1.3 Verify camera bounds and `WORLD_W` still work with the fixed base resolution; adjust camera setup if it relied on the dynamic viewport size.
- [ ] 1.4 Confirm on desktop (large window), small desktop window, and a phone in landscape that the player and enemies look proportionally the same and that resizing the window re-scales without reload.

## 2. Touch overlay markup and styles

- [x] 2.1 Add a `.touch-controls` overlay to `play.html` with two button clusters: a movement pad (left/right) on the right-bottom, and jump + shoot buttons on the left-bottom.
- [x] 2.2 Style buttons in `play.html` (or `css/style.css`) using `min(14vh, 14vw)` sizing, semi-transparent backgrounds, rounded shapes, and `touch-action: none; user-select: none;`.
- [x] 2.3 Hide the overlay by default; reveal it only when `body` has a `.touch` class.

## 3. Touch input wiring

- [x] 3.1 On page load, detect touch capability (`'ontouchstart' in window || navigator.maxTouchPoints > 0`) and add `body.touch` when true.
- [x] 3.2 Create a `touchInput = { left: false, right: false, jumpPressed: false, shootPressed: false }` object accessible to `MainScene`.
- [x] 3.3 Add `pointerdown` / `pointerup` / `pointercancel` listeners on each button that toggle the matching flag; for jump and shoot use one-shot `*Pressed` flags consumed in `update()`.
- [x] 3.4 In `MainScene.update()`, OR `touchInput.left/right` into the existing `left`/`right` checks, and OR the one-shot jump/shoot flags into the existing "just pressed" logic; clear the one-shots after reading.
- [x] 3.5 Reset all `touchInput` flags on `visibilitychange` to hidden, on `pointercancel`, and on `pointerleave` of a held button so the player can't get stuck moving.

## 4. Cross-device verification

- [ ] 4.1 Test on desktop with keyboard: movement, jump, shoot all still work (no regression).
- [ ] 4.2 Test on a phone in landscape: overlay appears, all four actions work, buttons remain reachable.
- [ ] 4.3 Test on a touchscreen laptop: both keyboard and touch work simultaneously.
- [ ] 4.4 Verify the overlay does not cover the player, the score, or the "Home" back-link.

## 5. Ship

- [ ] 5.1 Commit changes to `html/play.html` (and `css/style.css` if touched).
- [ ] 5.2 Confirm `python3 -m http.server 8000` and opening `html/play.html` via `file://` both still load the game.
