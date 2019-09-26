---
Title: Pac-Man
category: python
order: 8
description: Pacman game using the python language and turtle drawing library
---

In this activity, we will be creating a _Pac-Man_ maze game, where the user moves around a maze eating all the dots, while being pursued by four ghosts.

**Taken and adapted from:**
[Free Python Games](http://www.grantjenks.com/docs/freegames/pacman.html)

## 1. Setup

Go to <http://bit.ly/storm-coder-dojo-pacman> to use a Trinket project that we have already created as a starter project.  Hit remix (log into your account if you have one or create one to save your progress). 

We have added some utility functions for you in `utilities.py` as well as some helpful starter code in `main.py`.

## 2. Draw the maze

The starter project already has an array called `tiles` that contains our maze.  In the array, a `0` represents a wall, and a `1` represents a path where we will add a dot for pac-man to eat.

Create a function called `world` after the `# Add your code here` comment.  Remember to be very careful about the indentation of each line - for example, make sure the `path.update()` is lined up exactly under the `for`.

```python
def world():
    Screen().bgcolor('black')
    path.color('blue')

    for index in range(len(tiles)):
        tile = tiles[index]

        if tile > 0:
            x = (index % 20) * 20 - 200
            y = 180 - (index // 20) * 20
            square(x, y)

            if tile == 1:
                path.up()
                path.goto(x + 10, y + 10)
                path.dot(2, 'white')
    
    update()
```

This function sets the background of the screen to black, then uses the `path` turtle to draw a 20 pixel blue square for every tile with a value greater than zero.  If a tile value equals `1`, then we draw a white dot in the middle of the square.

We need to call this function.  At the end of your program, add a new line to call `world()` so that the last two lines look like this:

```python
world()
done()
```

## 3. Draw Pac-Man

We need to set a starting position and direction for Pac-Man.  At the top of the code, after the line that says `path = turtle.Turtle()`, add the following code:

```python
aim = vector(5, 0)
pacman = vector(-40, -80)
```

Then create a new function called `move()`.   Add the following code in the gap before the line that says `Screen().setup(420, 420, 370, 0)`:

```python
def move():
  clear()
  
  up()
  goto(pacman.x + 10, pacman.y + 10)
  dot(20, 'yellow')
  
  update()

  Screen().ontimer(move, 100)
```

Change the bottom of your program to look like this:

```python
world()
move()
done()
```

If you run the program, you should now see a yellow circle near the bottom of the maze.

## 4. Move Pac-Man

We need to create a new function to `change` the position of Pac-Man when we press a key.  In the gap after before the line that says `Screen().setup(420, 420, 370, 0)`, add the following function:

```python
def change(x, y):
    if valid(pacman + vector(x, y)):
        aim.x = x
        aim.y = y
```

Now remove the `#` from the start of the lines near the bottom of your program.  This will cause the program to listen for arrow keypresses, and update Pac-Man's direction.

We need to move Pac-Man on every screen update.  Change the `move()` function so it looks like this:

```python
def move():
  clear()
  
  if valid(pacman + aim):
      pacman.move(aim)
        
  up()
  goto(pacman.x + 10, pacman.y + 10)
  dot(20, 'yellow')
  
  update()

  Screen().ontimer(move, 100)
```

## 5. Increase the score

When Pac-Man moves over a dot, the score increases by one, and the dot is removed. Add the following code to the middle of the `move()` function, so that it looks like this:

```python
  if valid(pacman + aim):
      pacman.move(aim)
      
  index = offset(pacman)

  if tiles[index] == 1:
      tiles[index] = 2
      state['score'] += 1
      x = (index % 20) * 20 - 200
      y = 180 - (index // 20) * 20
      square(x, y)
        
  up()
```

This code changes a visited square to a `2` in our tiles array, and then updates the score by one point.

We need to write the score to the screen, so change the beginning of the `move()` function so that it looks like this:

```python
def move():
  writer.clear()
  writer.write(state['score'])
  clear()
```

## 6. Add the ghosts

First, we need to add the starting positions of our ghosts.  At the top of your code.   We already have their starting positions in the starter code.

Add the following code to your `move()` function, so that it looks like this:

```python
  dot(20, 'yellow')
  
  for point, course in ghosts:
    if valid(point + course):
        point.move(course)
    else:
      options = [
          vector(5, 0),
          vector(-5, 0),
          vector(0, 5),
          vector(0, -5),
      ]
      plan = choice(options)
      course.x = plan.x
      course.y = plan.y

    up()
    goto(point.x + 10, point.y + 10)
    dot(20, 'red')
    
  update()
```
Here, we move each ghost, as long as the new position is `valid()`.  We then choose a random direction (random `choice()`).

You will notice that the game never stops if we collide with a ghost. The final part of our program is a check at the end of the `move()` function.  Change the code so it looks like this:

```python
  update()
  
  for point, course in ghosts:
    if abs(pacman - point) < 20:
        return

  Screen().ontimer(move, 100)
```

The move function should now `return` if our Pac-Man hits a ghost, and the program will finish.

## Challenges

* Create a different maze
* Change the number of ghosts
* Change the starting position of Pac-Man
* Make the ghosts faster
* Make the ghosts smarter


## Full Code listing 

The final `main.py` code should look similar to this:

```python
from turtle import *
from utilities import *
from random import choice

path = Turtle()
writer = Turtle()

aim = vector(5, 0)
pacman = vector(-40, -80)

ghosts = [
    [vector(-180, 160), vector(5, 0)],
    [vector(-180, -160), vector(0, 5)],
    [vector(100, 160), vector(0, -5)],
    [vector(100, -160), vector(-5, 0)],
]

state = {'score': 0}

tiles = [
    0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
    0, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0,
    0, 1, 0, 0, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0,
    0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0,
    0, 1, 0, 0, 1, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 0, 0,
    0, 1, 1, 1, 1, 0, 1, 1, 0, 1, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0,
    0, 1, 0, 0, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0,
    0, 1, 0, 0, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0,
    0, 1, 1, 1, 1, 1, 1, 0, 0, 0, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0,
    0, 0, 0, 0, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 0, 1, 0, 0, 0, 0,
    0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 0, 0,
    0, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0,
    0, 1, 0, 0, 1, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0,
    0, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 0, 0, 0,
    0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0,
    0, 1, 1, 1, 1, 0, 1, 1, 0, 1, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0,
    0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0,
    0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0,
    0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
    0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
]

def square(x, y):
    "Draw square using path at (x, y)."
    path.hideturtle()
    path.up()
    path.goto(x, y)
    path.down()
    path.begin_fill()

    for count in range(4):
        path.forward(20)
        path.left(90)

    path.end_fill()
    
def offset(point):
    "Return offset of point in tiles."
    x = (floor(point.x, 20) + 200) / 20
    y = (180 - floor(point.y, 20)) / 20
    index = int(x + y * 20)
    return index

def valid(point):
    "Return True if point is valid in tiles."
    index = offset(point)

    if tiles[index] == 0:
        return False

    index = offset(point + 19)

    if tiles[index] == 0:
        return False

    return point.x % 20 == 0 or point.y % 20 == 0

# Add your code here

def world():
    Screen().bgcolor('black')
    path.color('blue')

    for index in range(len(tiles)):
        tile = tiles[index]

        if tile > 0:
            x = (index % 20) * 20 - 200
            y = 180 - (index // 20) * 20
            square(x, y)

            if tile == 1:
                path.up()
                path.goto(x + 10, y + 10)
                path.dot(2, 'white')
    
    update()

def move():
  writer.clear()
  writer.write(state['score'])
  clear()
  
  if valid(pacman + aim):
      pacman.move(aim)
      
  index = offset(pacman)

  if tiles[index] == 1:
      tiles[index] = 2
      state['score'] += 1
      x = (index % 20) * 20 - 200
      y = 180 - (index // 20) * 20
      square(x, y)
        
  up()
  goto(pacman.x + 10, pacman.y + 10)
  dot(20, 'yellow')
  
  for point, course in ghosts:
    if valid(point + course):
        point.move(course)
    else:
      options = [
          vector(5, 0),
          vector(-5, 0),
          vector(0, 5),
          vector(0, -5),
      ]
      plan = choice(options)
      course.x = plan.x
      course.y = plan.y

    up()
    goto(point.x + 10, point.y + 10)
    dot(20, 'red')
    
  update()
  
  for point, course in ghosts:
    if abs(pacman - point) < 20:
        return

  Screen().ontimer(move, 100)

def change(x, y):
    "Change pacman aim if valid."
    if valid(pacman + vector(x, y)):
        aim.x = x
        aim.y = y

Screen().setup(420, 420, 370, 0)
Screen().tracer(0, 0)
writer.hideturtle()
writer.goto(160, 160)
writer.color('white')
writer.write(state['score'])
Screen().listen()
hideturtle()
Screen().onkey(lambda: change(5, 0), 'Right')
Screen().onkey(lambda: change(-5, 0), 'Left')
Screen().onkey(lambda: change(0, 5), 'Up')
Screen().onkey(lambda: change(0, -5), 'Down')

world()
move()
done()
```

