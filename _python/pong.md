---
Title: Pong
category: python
order: 9
description: Pong game using the python language and turtle drawing library
---

In this activity, we will be creating a _Pong_ game, where two users control paddles that move up and down the screen, simulating a table tennis game.

**Taken and adapted from:**
[Free Python Games](http://www.grantjenks.com/docs/freegames/pong.html)

## 1. Setup

Go to <http://bit.ly/storm-coder-dojo-pong> to use a Trinket project that we have already created as a starter project.  Hit remix (log into your account if you have one or create one to save your progress). 

We have added some utility functions for you in `utilities.py` as well as the imports that you will need in `main.py`.

At the bottom of `main.py` add the following code to set up the playing area:

```python
Screen().setup(420, 420, 370, 0)
hideturtle()
Screen().tracer(0, 0)
Screen().listen()

done()
```

## 2. The ball

We will store the position of the ball in a vector `ball` which will keep track of the `x` and `y` coordinates of the ball.

We will store the direction of the ball in another vector `aim`, that contains the `x` and `y` movement of the ball in each frame.  Every time we draw a new frame, we will add the `aim` vector to the `ball` vector to get the new position.  

After the imports at the top of `main.py`, add the following code:

```python
ball = vector(0, 0)
aim = vector(5, 5)
```

Just after this, add a function to move and draw the ball every frame:

```python
def draw():
    clear()

    ball.move(aim)
    x = ball.x
    y = ball.y

    up()
    goto(x, y)
    dot(10)
    update()

    Screen().ontimer(draw, 50)
```

Finally, add one line of code near the bottom of  `main.py`, so that the last few lines of your program look like this:

```python
Screen().setup(420, 420, 370, 0)
hideturtle()
Screen().tracer(0, 0)
Screen().listen()
Screen().onkey(lambda: draw(), 'Space')   # this is the new line
done()
```

Run the program.  When you click on the output area, and hit `Space`, you should see the ball start in the centre of the screen, and move towards the top right corner.

## 3. Draw the paddles

In _Pong_, the paddles move up and down the screen, but cannot move left or right.  We only need to store the `y` value of their position on the screen.  We'll store this in a variable called `state` that starts both paddles in the middle of the screen.

Near the top of `main.py` add one new line so that your code looks like this:

```python
ball = vector(0, 0)
aim = vector(5, 5)
state = {1: 0, 2: 0}   # This is the new line of code
```

We will need a function to draw a rectangle for each paddle.  Add this code straight after the line you just added:

```python
def rectangle(x, y, width, height):
    up()
    goto(x, y)
    down()
    begin_fill()
    for count in range(2):
        forward(width)
        left(90)
        forward(height)
        left(90)
    end_fill()
```

Now add two lines to the draw function to actually draw the rectangles on the screen:

```python
def draw():
    clear()
    rectangle(-200, state[1], 10, 50)  # This line is new
    rectangle(190, state[2], 10, 50)   # This line is new

    ball.move(aim)
    x = ball.x
    y = ball.y
```

If you run the program, you will see two paddles drawn, but you can't move them yet.

## 4. Moving the paddles

When a player presses a key, we need to change the state of that player to move them to a new `y` position on the screen.

Directly above the line that says `def draw():`, add the following function:

```python
def move(player, change):
    state[player] += change
```

We now need to listen for key presses from each player.  Change the bottom of  `main.py` so that it looks like this:

```python
Screen().listen()
Screen().onkey(lambda: move(1, 20), 'w')    # This line is new
Screen().onkey(lambda: move(1, -20), 's')   # This line is new
Screen().onkey(lambda: move(2, 20), 'i')    # This line is new
Screen().onkey(lambda: move(2, -20), 'k')   # This line is new
Screen().onkey(lambda: draw(), 'Space')
done()
```

If you run the program, Player 1 can move using the `w` and `s` keys, and Player 2 can move using the `i` and `k` keys.

## 5. Handling collisions

There are a few where the ball can hit something.  

* The ball hits the top or bottom of the screen.  We simply reverse the `y` direction of the ball.
* The ball hits the left or right of the screen.  Here, we first have to check if we have hit a paddle.  In this case we simply reverse the `x` direction of the ball.  If not, the game is over.

Change the `draw` function so that it looks like this:

```python
def draw():
    clear()
    rectangle(-200, state[1], 10, 50)
    rectangle(190, state[2], 10, 50)

    ball.move(aim)
    x = ball.x
    y = ball.y

    up()
    goto(x, y)
    dot(10)
    update()

    # Beginning of new code
    if y < -200 or y > 200:
        aim.y = -aim.y

    if x < -185:
        low = state[1]
        high = state[1] + 50

        if low <= y <= high:
            aim.x = -aim.x
        else:
            return

    if x > 185:
        low = state[2]
        high = state[2] + 50

        if low <= y <= high:
            aim.x = -aim.x
        else:
            return
    # End of new code

    Screen().ontimer(draw, 50)
```

You now have a working game of _Pong_

## 6. Random starting direction

The trouble with this game is that the ball is always moving in a very predictable direction, because we set this at the start by saying `aim = vector(5, 5)`.  We can make this a bit more random.

At the top of your program, just after the imports, add the following code:

```python
from random import choice, random
from turtle import *
from utilities import *

# This is the new code
def random_value():
    return (3 + random() * 2) * choice([1, -1])
```

Now change the aim so it looks like this:

```python
ball = vector(0, 0)
aim = vector(random_value(), random_value())  # This line has changed
state = {1: 0, 2: 0}
```

The ball should now start moving in a random direction.

## Challenges

* Change the colour of the paddle or ball
* Change the speed of the ball
* Change the size of the paddles
* Add some randomness to the direction after the ball bounces off something
* Add another ball
* Think about how you would add a computer player


## Full listing

```python
from random import choice, random
from turtle import *
from utilities import *

def random_value():
    return (3 + random() * 2) * choice([1, -1])

ball = vector(0, 0)
aim = vector(random_value(), random_value())
state = {1: 0, 2: 0}

def move(player, change):
    state[player] += change

def rectangle(x, y, width, height):
    up()
    goto(x, y)
    down()
    begin_fill()
    for count in range(2):
        forward(width)
        left(90)
        forward(height)
        left(90)
    end_fill()

def draw():
    clear()
    rectangle(-200, state[1], 10, 50)
    rectangle(190, state[2], 10, 50)

    ball.move(aim)
    x = ball.x
    y = ball.y

    up()
    goto(x, y)
    dot(10)
    update()

    if y < -200 or y > 200:
        aim.y = -aim.y

    if x < -185:
        low = state[1]
        high = state[1] + 50

        if low <= y <= high:
            aim.x = -aim.x
        else:
            return

    if x > 185:
        low = state[2]
        high = state[2] + 50

        if low <= y <= high:
            aim.x = -aim.x
        else:
            return

    Screen().ontimer(draw, 50)

Screen().setup(420, 420, 370, 0)
hideturtle()
Screen().tracer(0, 0)
Screen().listen()
Screen().onkey(lambda: move(1, 20), 'w')
Screen().onkey(lambda: move(1, -20), 's')
Screen().onkey(lambda: move(2, 20), 'i')
Screen().onkey(lambda: move(2, -20), 'k')
Screen().onkey(lambda: draw(), 'Space')

done()
```



