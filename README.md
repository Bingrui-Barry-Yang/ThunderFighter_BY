# Thunder Fighter in Jack - Barry Yang
Thunder Fighter is a fighter jet shooting game written in Jack.

The player controls a fighter with the arrow keys, shoots automatically, dodges enemy bullets, clears enemy waves, and fights boss enemies. The game ends when the player loses all HP.

# Controls
1. Press `S` to start.
2. Use arrow keys to move.
3. Press `Space` to pause or resume.
4. Press `R` to restart.

# Structure
`Main.jack` creates the game object and starts the game loop.

`Game.jack` controls the main game state, including waves, spawning, score, pause/restart, high score, collision checks, and rendering.

`Player.jack` stores the player position, HP, movement, and fighter drawing.

`Enemy.jack` stores both normal enemy and boss state, including HP, movement, shooting, and drawing.

`Bullet.jack` stores bullet position, direction, owner, speed behavior, and drawing.

`Collision.jack` checks whether two objects touch.

# Program Flow
To avoid memory problems, the game reuses enemy and bullet objects instead of creating new ones forever.

The game starts on a title screen and waits for the player to press `S`. After that, the main loop repeatedly reads keyboard input, handles pause/restart, updates the player, spawns waves when the screen is clear, updates bullets and enemies, checks collisions, redraws the screen, and waits briefly with `Sys.wait`.

During play, pressing `Space` pauses the update logic until `Space` is pressed again. Pressing `R` resets the current run while keeping the highest score.

When the player's HP reaches 0, the game shows the game-over screen with the score and highest score. Pressing `R` starts a new run.

# Waves and Bosses
Normal enemies appear in waves. Each wave contains exactly four enemy jets, and the next wave starts only after all current enemies are gone. Enemy x positions are randomized while staying inside the screen.

The wave counter increases for both normal waves and boss waves. After every three normal enemy waves, the next wave is a boss wave instead of another normal wave.

Bosses are larger enemies with their own bitmap design, HP, random movement, and three-shot bullet firing from left, center, and right. The first boss has 20 HP, and each later boss has 5 more HP than the previous boss. Boss movement also changes by boss number so later boss paths are not identical.

# Enemy Evolution
Enemy difficulty evolves only after a boss is defeated. The first normal enemy waves use the base difficulty: slower bullets, longer shooting intervals, and six possible spawn columns.

Each defeated boss increases enemy bullet speed by 5% and decreases enemy shooting interval by 5%. Boss bullets use the same speed and interval evolution as normal enemy bullets.

Every two defeated bosses, normal enemy waves unlock one more possible spawn column, up to the screen-safe limit. This makes later waves less predictable while still keeping every enemy fully visible.

# HP, Score, and Records
The player starts with 3 HP. Defeating a boss gives the player `HP++`, increasing player HP by 1.

Normal enemies give 10 points. Bosses give 100 points. The game records the highest score during the session and shows a new-record message when the player beats it.
