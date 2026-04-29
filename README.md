# Thunder Fighter in Jack - Barry Yang
Thunder Fighter is a simple fighterjet shooting game.

The player controls a fighter and moves it with the keyboard. The fighter shoots bullets automatically. Enemies come from the top of the screen, move downward, stop at a certain height, and shoot back. The player gets points by shooting enemies. The game ends when the player loses all health points (HP).

# Structure
`Main` creates the game object and starts the game loop.

`Game.jack` is the main controller of the whole program. It updates the player & bullets & enemies (movements), score and collisions. It also shows "gameover" and controlls restart.

`Player.jack` stores the player position, HP, movement, and fighter drawing.

`Enemy.jack` stores enemy position, enemy shooting, and enemy drawing.

`Bullet.jack` stores bullet position, direction, owner, movement, and bullet drawing.

`Collision.jack` checks if two objects touch each other (hit or crash).

# About the game loop
1. Read keyboard input
2. Update player movement
3. Spawn bullets and enemies
4. Update bullets and enemies
5. Check collisions
6. Draw the whole screen again
7. Wait a short time with `Sys.wait`

To avoid memory problems(ERR^), the game reuses enemy and bullet objects instead of making new ones forever.

# Current Features
1. Player movement with arrow keys
2. Automatic player shooting
3. Enemy spawning from multiple fixed x positions
4. Enemy bullets shooting
5. Collision between player, enemy, and bullets
6. Score system
7. HP system
8. "Game over" screen
9. Restart with `R`


# Possible Future Plans & Features
1. Different types of enemies that are able to move in different directions instead of just moving downwards and stay there.
2. Faster and harder enemy waves, enemy wave counting
3. Boss enemy every 5 waves
4. Power-ups for player weapon, shields and special skills like bomb or hp++.
5. Menu and different fighters for user to choose.
6. Pause function during the game
7. Better rendering.
