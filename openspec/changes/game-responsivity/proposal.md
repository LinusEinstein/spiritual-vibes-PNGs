## Why

The mini-game in `play.html` is unplayable on touch devices (no on-screen controls) and looks tiny on desktop because game elements don't scale with the viewport. This blocks a core piece of the edutainment experience on the two device classes that matter most.

## What Changes

- Add on-screen touch controls when a touch input is detected: a movement pad on the right side, jump and shoot buttons on the left side.
- Wire those controls into the existing input handling so they drive the same actions as the keyboard.
- Make the Phaser game canvas and in-game elements scale to fill the available landscape viewport on desktop, while preserving the current good-looking sizing on mobile.
- Keep keyboard controls working unchanged.

## Capabilities

### New Capabilities
- `game-input`: How the player controls the game (keyboard + on-screen touch buttons), including layout, activation rules, and action mapping.
- `game-scaling`: How the game canvas and in-game elements size themselves to the viewport across desktop and mobile in landscape.

### Modified Capabilities
<!-- none — no existing specs in openspec/specs/ -->

## Impact

- `html/play.html` — game bootstrap, Phaser `Scale` config, input handling.
- `html/css/` (or inline styles in `play.html`) — styles/positioning for the on-screen control overlay.
- `html/assets/` — possibly small button icons (can also be CSS-drawn).
- No backend, no build step; runs as static files on GitHub Pages and via `file://`.
