---
Title: Connect four
category: python
order: 2
description: Connect 4 using the python language and turtle drawing library
---
In this activity, we will be re-making the connect four game with the Python programming language and using the Turtle Library.

Connect four is a game where two players place different counters in a frame one turn at a time. You win by getting four of your colour counters in a line (horizontal, vertical, or diagonal).

We will write a program that will draw out the frame that the counters go into and record the clicks of the players placing a counter.

**Taken and adapted from**
[Free Python Games](http://www.grantjenks.com/docs/freegames/connect.html)

## 1 – The frame

Go to trinkiet.io and start a new Python project: http://bit.ly/pythontrinket (log into your account if you have one or create one to save your progress). Then:

Import the `turtle` module into your project

```python
from turtle import *
```

Define a function `line` that takes the paramters `a`, `b`, `x`, `y`:

```python
def line(a, b, x, y):
    up()
    goto(a, b)
    down()
    goto(x, y)
```

This will be used to draw the verticle lines on our frame to make the columns clear.

Define a function `grid`:

```python
def grid():
    Screen().bgcolor('light blue')

    for x in range(-150, 200, 50):
        line(x, -200, x, 200)

    for x in range(-175, 200, 50):
        for y in range(-175, 200, 50):
            up()
            goto(x, y)
            dot(40, 'white')
    
    update()
```

This makes the background blue, then draws the vertical lines using the line function we just wrote and, finally, draws white dots to simulate the holes in a physical frame.

You can check your frame is drawing properly by writing the following (after and outside the function definition) and clicking run:

```python
Screen().setup(420, 420, 370, 0)
hideturtle()
Screen().tracer(0, 0)
grid()
```

## 2 – Placing a counter

All the code in this section should go below the `grid` function, and above the code starting `Screen().setup...`

There are two players, red and yellow. Each places a counter one after the other. We need to record whose turn it is and where the counter should go. Create two dictionaries, `turns` and `state`:

We can now use these to write the function to handle a player placing a counter. Define a function `tap` that takes two parameters `x` and `y`:

```python
def tap(x, y):
    player = state['player']
    rows = state['rows']

    row = int((x + 200) // 50)
    count = rows[row]

    x = ((x + 200) // 50) * 50 - 200 + 25
    y = count * 50 - 200 + 25

    up()
    goto(x, y)
    dot(40, player)
    update()

    rows[row] = count + 1
    state['player'] = turns[player]
```

This function:
- Checks which player should be placing a counter (yellow always starts).
- Works out which row the player has clicked. 
- Checks how many counters are already in that row – then places a counter in the space above.
- Finally it records how many counters are now in the row and changes the player state to be correct for the next turn.

## 3 – Final set up to play the game

We need to call the `tap` function when we click the screen. Right at the end of all the code put the following:

```python
Screen().onscreenclick(tap)
done()
```

That’s it. Now you should be able to play connect four against a friend.

### Solution in Trinket
<iframe src="https://trinket.io/embed/python/54fa682242" width="100%" height="600" frameborder="0" marginwidth="0" marginheight="0" allowfullscreen></iframe>

## 4 – Challenges

- Change the colours
- Draw squares instead of circles for the spaces
- Add logic to detect a full row
- Create a random computer player
- How would you detect a winner (advanced!)

### Solution with logic to detect full row and winner
`main.py`
```python
from turtle import *
from state import *
from check_win import is_win

def line(a, b, x, y):
    up()
    goto(a, b)
    down()
    goto(x, y)

def grid():
    Screen().bgcolor('light blue')
    for x in range(-150, 200, 50):
        line(x, -200, x, 200)

    for x in range(-175, 200, 50):
        for y in range(-175, 200, 50):
            up()
            goto(x, y)
            dot(40, 'white')

    update()
    
turns = {'red': 'yellow', 'yellow': 'red'}
state = State()

def tap(x, y):
    player = state.player
    slots = state.slots
    grid = state.grid

    slot = int((x + 200) // 50)
    count = slots[slot]
    
    if count >= len(grid):
      print "No more space, try another slot"
      return

    x = ((x + 200) // 50) * 50 - 200 + 25
    y = count * 50 - 200 + 25

    up()
    goto(x, y)
    dot(40, player)
    update()

    grid[len(grid) - count - 1][slot] = player
    last_played_cell = [len(grid) - count - 1, slot]
    if is_win(grid, player, last_played_cell, 4):
      print(str(player) + " wins")
      restart()
    slots[slot] = count + 1
    state.player = turns[player]

def restart():
  reset()
  hideturtle()
  grid()
  state.reset()
  Screen().onscreenclick(tap)
  done()

Screen().setup(420, 420, 370, 0)
hideturtle()
Screen().tracer(0, 0)
grid()
Screen().onscreenclick(tap)
done()
```
`state.py`
```python
class State:
  def __init__(self):
    self.player = 'yellow'
    self.slots = [0] * 8
    self.grid = [[None,None,None,None,None,None,None,None],
                 [None,None,None,None,None,None,None,None],
                 [None,None,None,None,None,None,None,None],
                 [None,None,None,None,None,None,None,None],
                 [None,None,None,None,None,None,None,None],
                 [None,None,None,None,None,None,None,None],
                 [None,None,None,None,None,None,None,None],
                 [None,None,None,None,None,None,None,None]]
  
  def reset(self):
    self.slots = [0] * 8
    self.grid = [[None,None,None,None,None,None,None,None],
                 [None,None,None,None,None,None,None,None],
                 [None,None,None,None,None,None,None,None],
                 [None,None,None,None,None,None,None,None],
                 [None,None,None,None,None,None,None,None],
                 [None,None,None,None,None,None,None,None],
                 [None,None,None,None,None,None,None,None],
                 [None,None,None,None,None,None,None,None]]
```
`win_check.py`
```python
def next_cell(cell, offset):
  return [cell[0] + offset[0], cell[1] + offset[1]]

def is_in_grid(grid_height, grid_width, cell):
  return (cell[0] >= 0 and
          cell[0] < grid_height and 
          cell[1] >= 0 and
          cell[1] < grid_width)
          
def count_diagonal(grid, last_played_cell, offset1, offset2):
  cell = last_played_cell
  grid_height = len(grid)
  grid_width = len(grid[0])
  count = 1
  
  while is_in_grid(grid_height, grid_width, next_cell(cell, [offset1[0], offset1[1]])):
    count += 1
    cell = next_cell(cell, [offset1[0], offset1[1]])
  cell = last_played_cell  
  
  while is_in_grid(grid_height, grid_width, next_cell(cell, [offset2[0], offset2[1]])):
    count += 1
    cell = next_cell(cell, [offset2[0], offset2[1]])
    
  return count  

def is_horizontal_win(grid, player, last_played_cell, win_score):
    score = 0
    for cell in grid[last_played_cell[0]]:
      if cell == player:
        score += 1
        if score >= win_score:
          return True
      else:
        score = 0
    return False
    
def is_vertical_win(grid, player, last_played_cell, win_score):
    score = 0
    for row in range(0, len(grid)):
      if grid[row][last_played_cell[1]] == player:
        score += 1
        if score >= win_score:
          return True
      else:
        score = 0
    return False
    
def is_diagonal_win(grid, player, last_played_cell, win_score, offset1, offset2):

  if count_diagonal(grid, last_played_cell, offset1, offset2) < win_score:
    return False
  
  cell = last_played_cell
  while is_in_grid(len(grid), len(grid[0]), next_cell(cell, offset1)):
    cell = next_cell(cell, offset1)
  
  score = 0
  for i in range(0, count_diagonal(grid, last_played_cell, offset1, offset2)):
    if grid[cell[0]][cell[1]] == player:
      score += 1
      if score >= win_score:
        return True
    else:
      score = 0
    cell = next_cell(cell, offset2)
  return False    
    
def is_win(grid, player, last_played_cell, win_score):
  if is_horizontal_win(grid, player, last_played_cell, win_score):
    return True
  if is_vertical_win(grid, player, last_played_cell, win_score):
    return True
  #Descending diagonal  
  if is_diagonal_win(grid, player, last_played_cell, win_score, [-1, -1], [1, 1]):
    return True
  #Ascending diagonal
  else:
    return is_diagonal_win(grid, player, last_played_cell, win_score, [-1, 1], [1, -1])
```

### Solution in trinket
<iframe src="https://trinket.io/embed/python/ae72b24291" width="100%" height="600" frameborder="0" marginwidth="0" marginheight="0" allowfullscreen></iframe>