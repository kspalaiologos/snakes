
# Snakes: Round 1

The challenge is played on a 32x16 board. Two AIs are placed in opposite corners of the map - (0, 0) and (31, 15).
The board data consists of five unique ASCII characters:
- `X` - the territory of player X
- `O` - the territory of player O
- `*` - the player X
- `@` - the player O
- `.` - empty space

The game is played in turns. The game ends after 1024 turns, and the player who holds more territory at the
end of turn 1024 wins. The game yields the winning side a point, while the losing side keeps their amount of
points intact. A draw gives both players half a point.

Each turn, either of the bots can move upwards, downwards, left or right. Moving outside of the board is considered a
no-operation. After the move is completed, the new square is considered bot's territory. The corners (0, 0) and (31, 15)
are considered respective player's territories. When a bot creates a closed curve either with its territory, or the board
walls and the territory, the interior of it is marked as its territory. If the enemy bot was present inside of this
territory, it loses instantly.

## APIs

The code must not interfere with the event host, the opposing bot and may not declare, read or write global variables or explicitly modify the board.
Any signal sent by the bot (SIGSEGV, SIGFPE) counts as a loss for the said bot.

Create a function called `bot` which takes a 2-D, 32x16 array of characters as it's input representing the board
and the character it plays. Return either 'L', 'R', 'U', or 'D'.
Define a string constant containing the name of your bot:

```c
int bot(int board[16][32], char player) {
    ...
}

char * bot_name = ...;
```

## Examples

`random_vs_random.txt` contains a log printed by the host while playing a game of two bots making random moves.

The game starts.

```
*...............................
................................
................................
................................
................................
................................
................................
................................
................................
................................
................................
................................
................................
................................
................................
...............................@
```

After 10 turns, both players have moved 5 times up/down and 5 times left/right.
```
X...............................
X...............................
X...............................
X...............................
X...............................
XXXXX*..........................
................................
................................
................................
................................
..........................@.....
..........................O.....
..........................O.....
..........................O.....
..........................O.....
..........................OOOOOO
```

After 4 more turns, both of the players move 4 times towards the wall.

```
X...............................
X....*..........................
X....X..........................
X....X..........................
X....X..........................
XXXXXX..........................
................................
................................
................................
................................
..........................OOOO@.
..........................O.....
..........................O.....
..........................O.....
..........................O.....
..........................OOOOOO
```

Now, when they make one more move towards the wall, the area is theirs.

```
XXXXX*..........................
XXXXXX..........................
XXXXXX..........................
XXXXXX..........................
XXXXXX..........................
XXXXXX..........................
................................
................................
................................
................................
..........................OOOOO@
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO
```

A few more moves later:

```
XXXXXXXXXXX*....................
XXXXXX..........................
XXXXXX..........................
XXXXXX..........................
XXXXXX.........................@
XXXXXX.........................O
...............................O
...............................O
...............................O
...............................O
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO

XXXXXXXXXXXX....................
XXXXXX.....X....................
XXXXXX.....X....................
XXXXXX.*XXXX....................
XXXXXX......................OOOO
XXXXXX......................O..O
............................O..O
............................O@.O
...............................O
...............................O
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO
```

The players are about to mark another bit of territory:

```
XXXXXXXXXXXX....................
XXXXXXXXXXXX....................
XXXXXXXXXXXX....................
XXXXXX*XXXXX....................
XXXXXX......................OOOO
XXXXXX......................OOOO
............................OOOO
............................OO@O
...............................O
...............................O
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO
```

Let's witness player O committing suicide:

```
XXXXXXXXXXXX....................
XXXXXXXXXXXX....................
XXXXXXXXXXXX....................
XXXXXXXXXXXX....................
XXXXXXX.....................OOOO
XXXXXXXXXXXXXX*.............OOOO
............................OOOO
............................OOOO
.................@OOOOOOOOOOOOOO
...............................O
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO

XXXXXXXXXXXX....................
XXXXXXXXXXXX....................
XXXXXXXXXXXX....................
XXXXXXXXXXXX....................
XXXXXXX..........@..........OOOO
XXXXXXXXXXXXXX...O..........OOOO
.............X...O...*......OOOO
.............XXXXXXXXX......OOOO
.................OOOOOOOOOOOOOOO
...............................O
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO

XXXXXXXXXXXX....................
XXXXXXXXXXXX.........*..........
XXXXXXXXXXXX.........X..........
XXXXXXXXXXXX..@OOO...X..........
XXXXXXX..........O...X......OOOO
XXXXXXXXXXXXXX...O...X......OOOO
.............X...O...X......OOOO
.............XXXXXXXXX......OOOO
.................OOOOOOOOOOOOOOO
...............................O
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO
..........................OOOOOO
```

On the next move, player X moves up and ends the game, as player O will suffocate in X's territory.
