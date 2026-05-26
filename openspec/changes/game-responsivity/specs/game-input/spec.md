## ADDED Requirements

### Requirement: Keyboard controls remain available
The game SHALL continue to accept keyboard input for movement, jump, and shoot on any device that has a keyboard, regardless of whether touch controls are also shown.

#### Scenario: Desktop player uses keyboard
- **WHEN** a player on a desktop browser presses the keyboard keys mapped to move, jump, or shoot
- **THEN** the game responds with the corresponding action, identical to current behavior

### Requirement: Touch controls appear on touch-capable devices
The game SHALL display an on-screen control overlay when the device supports touch input, and SHALL hide the overlay on devices that do not.

#### Scenario: Mobile player opens the game
- **WHEN** a player opens `play.html` on a touch-capable device (e.g. phone, tablet)
- **THEN** an on-screen control overlay is visible on top of the game canvas

#### Scenario: Desktop player opens the game
- **WHEN** a player opens `play.html` on a desktop browser with no touch support
- **THEN** no on-screen control overlay is visible

### Requirement: Control layout follows thumb ergonomics
The on-screen control overlay SHALL place movement controls on the right side of the screen and the jump and shoot buttons on the left side, anchored near the bottom so both thumbs can reach them when the device is held in landscape.

#### Scenario: Layout of the overlay
- **WHEN** the touch overlay is shown
- **THEN** the movement control is anchored to the right-bottom area and the jump and shoot buttons are anchored to the left-bottom area

### Requirement: Touch controls drive the same actions as the keyboard
Pressing an on-screen button SHALL trigger the same in-game action as the corresponding keyboard key, including while the button is held.

#### Scenario: Holding the move-right button
- **WHEN** the player presses and holds the right-movement control
- **THEN** the player character moves right continuously until the control is released

#### Scenario: Tapping the jump button
- **WHEN** the player taps the jump button
- **THEN** the player character jumps once, matching the keyboard jump behavior

#### Scenario: Tapping the shoot button
- **WHEN** the player taps the shoot button
- **THEN** the player character fires, matching the keyboard shoot behavior

### Requirement: Touch controls do not interfere with the game view
The control overlay SHALL be visually unobtrusive and MUST NOT block essential gameplay elements such as the player, enemies, or score.

#### Scenario: Overlay over gameplay
- **WHEN** the touch overlay is visible during play
- **THEN** the buttons are semi-transparent and positioned so they do not cover the player character or score area
