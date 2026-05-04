# Thunder Fighter in Jack - Barry Yang
Thunder Fighter is a fighter jet shooting game written in Jack.

The player controls a fighter with the arrow keys, shoots automatically, dodges enemy bullets, clears enemy waves, and fights boss enemies. The game ends when the player loses all HP.

# Compile and Run
Compile the project folder with a Jack compiler, then load the generated VM files in the Nand2Tetris VM Emulator.

Example:

```sh
JackCompiler ThunderFighter_BY
```

If using the course compiler script:

```sh
python3 JackCompiler_BY.py ThunderFighter_BY
```

After compiling, open the compiled project folder in the VM Emulator and run `Main.main`.

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

# Class Relationships
`Main` only creates and starts `Game`.

`Game` owns the `Player`, the enemy pool, and the bullet pool. It is the only class that decides when to spawn waves, when to fire bullets, when to update score, and when the game is over.

`Enemy` represents both normal enemies and bosses. The `boss` flag changes HP, size, drawing, movement, and shooting behavior.

`Bullet` is shared by both the player and enemies. The `owner` value separates player bullets from enemy bullets during collision checks.

`Collision` is stateless and is called by `Game` whenever two rectangular hit boxes need to be compared.

# Program Flow
To avoid memory problems, the game reuses enemy and bullet objects instead of creating new ones forever.

The game starts on a title screen and waits for the player to press `S`. After that, the main loop repeatedly reads keyboard input, handles pause/restart, updates the player, spawns waves when the screen is clear, updates bullets and enemies, checks collisions, redraws the screen, and waits briefly with `Sys.wait`.

During play, pressing `Space` pauses the update logic until `Space` is pressed again. Pressing `R` resets the current run while keeping the highest score.

When the player's HP reaches 0, the game shows the game-over screen with the score and highest score. Pressing `R` starts a new run.

# Waves and Bosses
Normal enemies appear in waves. Each wave contains exactly four enemy jets, and the next wave starts only after all current enemies are gone. Enemy x positions are randomized while staying inside the screen.

The wave counter increases for both normal waves and boss waves. After every four normal enemy waves, the next wave is a boss wave instead of another normal wave.

Bosses are larger enemies with their own bitmap design, HP, random movement, and three-shot bullet firing from left, center, and right. The first boss has 50 HP, and each later boss has 20 more HP than the previous boss. Boss movement also changes by boss number so later boss paths are not identical.

# Enemy Evolution
Enemy difficulty evolves only after a boss is defeated. The first normal enemy waves use the base difficulty: slower bullets, longer shooting intervals, and ten possible spawn columns.

Each defeated boss increases enemy bullet speed by 8% and decreases enemy shooting interval by 8%. Boss bullets use the same speed and interval evolution as normal enemy bullets.

Each defeated boss unlocks two more possible spawn columns for normal enemy waves, up to a maximum of 20. This makes later waves less predictable while still keeping every enemy fully visible.

# HP, Score, and Records
The player starts with 3 HP. Defeating a boss gives the player `HP++`, increasing player HP by 1.

Normal enemies give 10 points. Bosses give points equal to their HP. The game records the highest score during the session and shows a new-record message when the player beats it.

# Subtle Parts
The game uses fixed object pools for bullets and enemies. This avoids creating new objects during play and helps prevent memory errors. `MAX_BULLETS` is currently 30, so extra shots are skipped if all bullet slots are active.

Rendering mostly uses full-screen redraw. Each frame clears the screen and redraws active objects and HUD text. This is simpler than erasing individual sprites, because the game does not need to remember what was underneath a moving object.

Some sprites use direct screen memory drawing, especially enemies and bosses. This is fast, but it means positions should stay screen-safe and mostly aligned to memory word boundaries. The player was changed to pixel-based `Screen` drawing so its movement can be smoother.

Enemy bullet speed uses integer movement plus bonus movement timing. This approximates percentage-based speed increases even though Jack does not support floating point numbers.

Even-numbered waves use angled enemy bullet patterns. Every third counted wave, including boss waves, uses a wider left-and-right bullet angle. The angle is approximated by occasionally shifting bullets left or right while they move downward.

# Known Issues and Limitations
The high score is kept only while the program is running. It is not saved to a file.

The random behavior is pseudo-random and based on simple integer seed updates, not a true random number generator.

The boss bitmap is large and drawn with many `Memory.poke` calls, which makes it harder to edit than the player drawing.

Full-screen redraw is reliable but may be slower than partial redraw.

Keyboard input is polled once per frame, so extremely quick taps may still be missed.

# Future Work
Improve player drawing and movement further, possibly with a more detailed pixel-based sprite.

Add different enemy types with different movement patterns.

Add power-ups besides boss `HP++`, such as shields or stronger player bullets.

Add saved high score support if persistent storage is allowed.

Add more boss attack patterns and clearer visual feedback when the boss is hit.
