## ADDED Requirements

### Requirement: Game assumes landscape orientation
The game SHALL be designed and laid out for a landscape aspect ratio and is not required to render correctly in portrait orientation.

#### Scenario: Landscape viewport
- **WHEN** the game is opened in a landscape viewport
- **THEN** the game canvas and HUD are laid out using the full available width and height without empty side bars beyond what the aspect ratio requires

### Requirement: Game canvas fills the available viewport
The Phaser canvas SHALL resize to fit the available viewport area on both desktop and mobile, preserving the game's intended aspect ratio (letterboxing if necessary).

#### Scenario: Large desktop window
- **WHEN** the game is opened in a large desktop browser window
- **THEN** the canvas scales up to fill the window (preserving aspect ratio), so the game does not appear tiny in the corner

#### Scenario: Mobile in landscape
- **WHEN** the game is opened on a mobile device held in landscape
- **THEN** the canvas fills the available viewport just as it does today, with no regression in mobile sizing

#### Scenario: Window is resized
- **WHEN** the player resizes the browser window or rotates the device
- **THEN** the canvas re-scales to fit the new viewport without reloading the page

### Requirement: In-game elements scale with the canvas
Sprites, text, and other in-game elements SHALL scale proportionally with the canvas size so that their relative size and on-screen positioning are consistent across viewport sizes.

#### Scenario: Player sprite on desktop vs. mobile
- **WHEN** the same game runs on a 1920×1080 desktop window and on a 720×360 mobile viewport
- **THEN** the player sprite occupies a comparable fraction of the screen on both, rather than appearing dramatically smaller on desktop

### Requirement: Touch overlay scales with the viewport
The on-screen touch controls (when present) SHALL scale and reposition with the viewport so they remain reachable and appropriately sized on any landscape screen.

#### Scenario: Overlay on a small phone vs. a tablet
- **WHEN** the touch overlay is shown on screens of different sizes
- **THEN** the buttons remain comfortably sized for a thumb and stay anchored to the left-bottom and right-bottom areas of the visible game area
