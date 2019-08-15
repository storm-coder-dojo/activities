---
Title: Connect four
category: python
order: 1
layout: page
---

# Connect four

In this activity, we will be re- making the connect four game with the Python programming language and using the Turtle Library.

Connect four is a game where two players place different counters in a frame one turn at a time. You win by getting four of your colour counters in a line (horizontal, vertical, or diagonal).

We will write a program that will draw out the frame that the counters go into and record the clicks of the players placing a counter.

## 1 – The frame

Go to trinkiet.io and start a new Python project: http://bit.ly/pythontrinket (log into your account if you have one or create one to save your progress). Then:

- Import the `turtle` module into your project

```python
from turtle import *
```

- Define a function `line`

```python
def line(a, b, x, y):
    up()
    goto(a, b)
    down()
    goto(x, y)
```

This will be used to draw the verticle lines on our frame to make the columns clear.

- Define a function `grid`:

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
Screen().tracer(False)
grid()
```

## 2 – Placing a counter

All the code in this section should go below the grid function, and above the code starting `Screen().setup...`

There are two players, red and yellow. Each places a counter one after the other. We need to record whose turn it is and where the counter should go. Create two dictionaries, turns and state:

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

This checks which player should be placing a counter (yellow always starts), works out which row the player has clicked. Checks how many counters are already in that row – then places a counter in the space above. Finally it records how many counters are now in the row and changes the player state to be correct for the next turn.

## 3 – Final set up to play the game

We now need to make the tap function get triggered so we need to call the function when the screen is clicked. Right at the end of all the code put the following:

```python
Screen().onscreenclick(tap)
done()
```

That’s it. Now you should be able to play connect four against a friend.

## 4 – Challenges

* Change the colours
* Draw squares instead of circles for the spaces
* Add logic to detect a full row
* Create a random computer player
* How would you detect a winner (advanced!)

## 5 – Whole Solution

```python
from turtle import *

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
state = {'player': 'yellow', 'rows': [0] * 8}

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

Screen().setup(420, 420, 370, 0)
hideturtle()
Screen().tracer(False)
grid()
Screen().onscreenclick(tap)
done()
```

(taken and adapted from Free Python Games: http://www.grantjenks.com/docs/freegames/#connect)
