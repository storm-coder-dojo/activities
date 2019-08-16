---
Title: Noughts and crosses
category: python
order: 1
description: Noughts and crosses using the python language and turtle drawing library
---
In this activity, we will be re-making Noughts and Crosses with the
Python programming language and using the Turtle Library.

Noughts and Crosses is a game where two players place an X or O in a 3
by 3 grid one turn at a time. You win by getting three of your marks in
a line (horizontal, vertical, or diagonal).

We will write a program that will draw out the grid that the marks are
drawn on and record the clicks of the players drawing a mark.

**Taken and adapted from:**
[Free Python Games](http://www.grantjenks.com/docs/freegames/tictactoe.html)

## 1-- The grid

Go to trinkiet.io and start a new Python project:
<http://bit.ly/pythontrinket> (log into your account if you have one
or create one to save your progress). Then:

-   Import all the classes from the `turtle` module into your project

```python
from turtle import *
```

Define a function `line` that takes parameters `a`, `b`, `x`, and `y`:

```python
def line(a, b, x, y):
    up()
    goto(a, b)
    down()
    goto(x, y)
```
This will draw the lines of our grid

-   Define a function `grid`:

```python
def grid():
    line(-67, 200, -67, -200)
    line(67, 200, 67, -200)
    line(-200, -67, 200, -67)
    line(-200, 67, 200, 67)
```
This draws the vertical and horizontal lines using the line function we just wrote.

You can check your frame is drawing properly by writing the following (after and outside the function definition) and clicking run:

```python
Screen().setup(420, 420, 370, 0)
hideturtle()
Screen().tracer(0,0)
grid()
update()
```

## 2 -- Drawing a mark

All the code in the following sections should go below the `grid` function
and *before* the block starting `Screen()`... unless otherwise noted

Define a function `drawx` that takes the parameters `x` and `y` that will draw
the cross mark for the X player:

```python
def drawx(x, y):
    line(x, y, x + 133, y + 133)
    line(x, y + 133, x + 133, y)
```

Define a function `drawo` that takes the parameters `x` and `y` that will draw
the nought mark for the O player:

```python
def drawo(x, y):
    up()
    goto(x + 67, y + 5)
    down()
    circle(62)
```

## 3 -- Remembering which player goes when

There are two players, nought and cross. Each draws a mark one after the
other. We need to record whose turn it is. Create a dictionary, `state`
and a list `players`:

```python
state = {'player': 0}
players = [drawx, drawo]
```

## 4 -- A little maths

We're going to record where the user clicks the screen but we'll need to
get a location that matches up with the grid's squares each time. Define
a function `floor` that takes a parameter `value`.

```python
def floor(value):
    return ((value + 200) // 133) * 133 - 200
```

## 5 -- Tap it

We now have everything we need to write a function that organises
drawing the correct type of mark in the right place. Define a function
`tap` that takes the parameters `x` and `y`:

```python
def tap(x, y):
    x = floor(x)
    y = floor(y)
    player = state['player']
    draw = players[player]
    draw(x, y)
    update()
    state['player'] = not player
```

This checks which player should be placing a counter (cross always
starts), draws the correct mark and finally changes the player for the
next turn.

## 6 -- Final set up to play the game

We now need to call the tap function when the screen is clicked. Right
at the end of all the code put the following (after the block beginning
`Screen()`... finishing `update()`):

```python
Screen().onscreenclick(tap)
done()
```

That's it. Now you should be able to play Noughts and Crosses against a
friend.

## 7-- Challenges

-   Change the colours

-   Add logic to prevent drawing several marks on the same square

-   Create a random computer player

-   How would you detect a winner (advanced!)

## Some challenge solutions

### Add logic to prevent drawing several marks on the same square

`main.py`

```python
from turtle import *
from state import *

def line(a, b, x, y):
    up()
    goto(a, b)
    down()
    goto(x, y)

def draw_grid():
    "Draw tic-tac-toe grid."
    line(-67, 200, -67, -200)
    line(67, 200, 67, -200)
    line(-200, -67, 200, -67)
    line(-200, 67, 200, 67)

def drawx(x, y):
    "Draw X player."
    line(x, y, x + 133, y + 133)
    line(x, y + 133, x + 133, y)

def drawo(x, y):
    "Draw O player."
    up()
    goto(x + 67, y + 5)
    down()
    circle(62)

def floor(value):
    "Round value down to grid with square size 133."
    return ((value + 200) // 133) * 133 - 200

state = State()

players = [drawx, drawo]


def tap(x, y):
    "Draw X or O in tapped square."
    x = floor(x)
    y = floor(y)
    player = state.player
    grid = state.grid;
    item = state.get_grid_item(x, y)
    
    if(grid[item[0]][item[1]] is not None):
      print "square already taken"
      return
    grid[item[0]][item[1]] = player
    draw = players[player]
    draw(x, y)
    update()
    state.player = not player
  
Screen().setup(420, 420, 370, 0)
hideturtle()
Screen().tracer(0, 0)
draw_grid()
update()
Screen().onscreenclick(tap)
done()
```

`state.py`

```python
class State:
  def __init__(self):
    self.player = 0
    self.grid = [[None, None, None],
                 [None, None, None],
                 [None, None, None]]
  
  def get_grid_item(self, x, y):
    dic = {
      (-200,66):[0, 0],
      (-67,66):[0, 1],
      (66,66):[0, 2],
      (-200,-67):[1, 0],
      (-67,-67):[1, 1],
      (66,-67):[1, 2],
      (-200,-200):[2, 0],
      (-67,-200):[2, 1],
      (66,-200):[2, 2]
    }
    return dic[(x, y)]
```

### How would you detect a winner (advanced!)

`main.py`

```python
from turtle import *
from state import *
from check_win import *

def line(a, b, x, y):
    up()
    goto(a, b)
    down()
    goto(x, y)

def draw_grid():
    "Draw tic-tac-toe grid."
    line(-67, 200, -67, -200)
    line(67, 200, 67, -200)
    line(-200, -67, 200, -67)
    line(-200, 67, 200, 67)

def drawx(x, y):
    "Draw X player."
    line(x, y, x + 133, y + 133)
    line(x, y + 133, x + 133, y)

def drawo(x, y):
    "Draw O player."
    up()
    goto(x + 67, y + 5)
    down()
    circle(62)

def floor(value):
    "Round value down to grid with square size 133."
    return ((value + 200) // 133) * 133 - 200

state = State()

def tap(x, y):
    "Draw X or O in tapped square."
    Screen().onscreenclick(None)
    x = floor(x)
    y = floor(y)
    player = state.player
    grid = state.grid;
    item = state.get_grid_item(x, y)
    
    if(grid[item[0]][item[1]] is not None):
      print "square already taken"
      return
    grid[item[0]][item[1]] = player
    if(player == state.players[0]):
      drawx(x, y)
    else:
      drawo(x, y)
    update()
    if(is_win(grid, item[0], item[1])):
      print("Player "+ str(player) + " wins")
      restart()
      return
    state.player = state.switch_player(player)
    Screen().onscreenclick(tap)

def restart():
  Screen().reset()
  hideturtle()
  state.reset()
  draw_grid()
  update()
  Screen().onscreenclick(tap)
  done()

  
Screen().setup(420, 420, 370, 0)
hideturtle()
Screen().tracer(0, 0)
draw_grid()
update()
Screen().onscreenclick(tap)
done()
```
`check_win.py`

```python
def is_win(grid, row, column):
  if grid[0][column] == grid[1][column] == grid[2][column]:
    return True
  
  if grid[row][0] == grid[row][1] == grid[row][2]:
    return True
  
  if row == column and grid[0][0] == grid[1][1] == grid[2][2]:
    return True
  
  if row + column == 2 and grid[0][2] == grid[1][1] == grid[2][0]:
    return True
  
  return False 
```
`state.py`

```python
class State:
  def __init__(self):
    self.players = ["X", "O"]
    self.player = self.players[0]
    self.grid = [[None, None, None],
                 [None, None, None],
                 [None, None, None]]
  
  def reset(self):
    self.player = self.switch_player(self.player)
    self.grid = [[None, None, None],
                 [None, None, None],
                 [None, None, None]]
                 
  def get_grid_item(self, x, y):
    dic = {
      (-200,66):[0, 0],
      (-67,66):[0, 1],
      (66,66):[0, 2],
      (-200,-67):[1, 0],
      (-67,-67):[1, 1],
      (66,-67):[1, 2],
      (-200,-200):[2, 0],
      (-67,-200):[2, 1],
      (66,-200):[2, 2]
    }
    return dic[(x, y)]
  
  def switch_player(self, player):
    if(player == self.players[0]):
      return self.players[1]
    else:
      return self.players[0]
```