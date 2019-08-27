---
Title: Snake
category: python
order: 6
description: Snake game using the python language and turtle drawing library
---
In this activity, we will be creating a _Snake_ game, where the player controls a 
snake that grows every time it eats food.  If the snake crosses over itself, then the 
game is over.

**Taken and adapted from:**
[Free Python Games](http://www.grantjenks.com/docs/freegames/snake.html)

## 1-- Setup the screen

Go to trinkiet.io and start a new Python project:
<http://bit.ly/pythontrinket> (log into your account if you have one
or create one to save your progress). Then:

-   Import all the classes from the `turtle` module into your project

```python
from turtle import *
```
Beneath this (leave a few lines as a gap, as we will be putting more code here later),
add some code to set up the screen size, and the turtle for drawing:

```python
Screen().setup(420, 420, 370, 0)
hideturtle()
Screen().tracer(0, 0)

done()
```
The first line sets up the screen size that we will be using to play the game.  We then 
hide the turtle so that we cannot see it while the screen is redrawing.  The `Screen().tracer(0, 0)` 
means that the drawing all happens in the background, and we will need to call `update()` to 
see any changes on screen.  This will make our game run faster.

## 2-- Create the snake

We will store the positions of all the segments of our snake in an array of coordinates, and then loop
through each segment and draw a square at that position.

In the gap after the `import` line, add the following function that draws a square at position (x, y). 

```python
snake = [[10, 0]]

def draw_square(x, y, size, name):
    up()
    goto(x, y)
    down()
    color(name)
    begin_fill()

    for count in range(4):
      forward(size)
      left(90)

    end_fill()
```

Then, after this, add another function that we will use to move and draw the snake:

```python
def move():
  clear()
  
  for body in snake:
    draw_square(body[0], body[1], 9, 'black')

  update()
  Screen().ontimer(move, 100)
```

This function first clears the existing screen, then loops through every segment of our snake
and draws a black square in the background.   The call to `update()` then displays all of the 
background drawing.  The final line sets a timer of 100 milliseconds before calling the `move` function 
again.

We need to make sure this function is called, so just above the `done()` at the bottom of the code, add the 
following:

```python
move()
```

## 3-- Move the snake

Our snake will continue to move in one direction until we press an arrow key.  We need another variable
to store the current direction of the snake.

At the top of your code, add the following code after the `snake = [[10, 0]]` line:

```python
aim = [0, -10]
```

These are coordinates that say our snake will start moving down the screen.

Immediately after this, add a function that will change the direction of our snake:

```python
def change(x, y):
  aim[0] = x
  aim[1] = y
```

We now need some code to monitor for our player pressing the arrow keys to change the direction of 
our snake.  At the bottom of your code, just before the `move()` call, add the following code:

```python
Screen().listen()
Screen().onkey(lambda: change(10, 0), 'Right')
Screen().onkey(lambda: change(-10, 0), 'Left')
Screen().onkey(lambda: change(0, 10), 'Up')
Screen().onkey(lambda: change(0, -10), 'Down')
```

Now our program will `listen` for our player pressing a key, and if it detects one of the four arrow
keys, it will `change` the direction.

Finally, we need to update the `move` function to update the position of the snake depending on the 
current settings in the `aim` variable.  At the beginning of the `move` function, before it calls `clear()`,
add the following code:

```python
  head = [snake[-1][0] + aim[0], snake[-1][1] + aim[1]]

  snake.append(head)
  
  snake.pop(0)
```

We create a new segment of the snake (`head`) by adding the values in our `aim` variable to the last
item in our list of snake segments (`snake[-1]`).  We then add this (`append`) to the end of our list of segments,
and finally remove the end of the snake (`pop`).  This should leave us with a snake of the same length (currently one square),
but that has moved one square forward.

Try running your program.  Use the arrow keys to change direction.  After clicking run, you will need to click on the 
output window so that the program can start listening for your keypresses.

## 4-- Check for the edge of the screen

Our game finishes if the head of our snake goes off of the screen.  We can do this with a simple check in our 
`move` function.  Start by creating a function that can check if a given coordinate is inside the screen.  Add the following
code just before the `move` function:

```python
def inside(head):
    return -200 < head[0] < 190 and -200 < head[1] < 190
```

If we pass the head of our snake into this function, it will check whether the x and y coordinates are inside the boundaries
of the screen.  We need to call this within our `move` function.  Add the following code just before the `snake.append(head)` call:

```python
  if not inside(head):
    draw_square(head[0], head[1], 9, 'red')
    update()
    return
```

If the head of the snake is _not_ inside the screen, we will draw a red square for the head, then `update` the screen, and `return` 
from the `move` function, ending the game.

## 5-- Adding food

As our snake moves around the screen, we will randomly add squares of food.  Every time we eat food, our snake will increase in
length by one segment.  We need a variable to store the position of the current item of food.  At the top of our code, add the following
line after `aim = [0, -10]`:

```python
food = [20, 0]
```

Now we need to check if the head of the snake is on a food square.  If it is, our snake eats it, and we create a new square 
at a random position.  To extend our snake, we can ignore the call to `snake.pop(0)`.

At the top of your code, add another import:

```python
from random import randrange
```

In the `move` function, replace the call to `snake.pop(0)` so that it looks like this:

```python
  if head == food:
    food[0] = randrange(-15, 15) * 10
    food[0] = randrange(-15, 15) * 10
  else:
    snake.pop(0)
```
We also need to draw the food on the screen.  After the call to `clear()`, add the following line:

```python
draw_square(food[0], food[1], 9, 'green')
```

Our snake should now move around the screen, and extend by one segment every time we eat some food.

## 6-- Check we don't cross our tail

The only thing left is to check that the head of the snake doesn't cross the tail.  We have a list of all our snake
segments stored in `snake`, so all we need to do is check if the `head` is in that collection.

Change the line `if not inside(head):` so that it looks like this:

```python
  if not inside(head) or head in snake:
```

Our game is now complete!

## Full Code listing

Your final code should be similar to this:

```python
from turtle import *
from random import randrange

snake = [[10, 0]]
aim = [0, -10]
food = [20, 0]

def change(x, y):
  aim[0] = x
  aim[1] = y
  
def draw_square(x, y, size, name):
    up()
    goto(x, y)
    down()
    color(name)
    begin_fill()

    for count in range(4):
      forward(size)
      left(90)

    end_fill()
  
def inside(head):
    return -200 < head[0] < 190 and -200 < head[1] < 190
    
def move():
  head = [snake[-1][0] + aim[0], snake[-1][1] + aim[1]]

  if not inside(head) or head in snake:
    draw_square(head[0], head[1], 9, 'red')
    update()
    return

  snake.append(head)
  
  if head == food:
    food[0] = randrange(-15, 15) * 10
    food[0] = randrange(-15, 15) * 10
  else:
    snake.pop(0)
    
  clear()
  
  for body in snake:
    draw_square(body[0], body[1], 9, 'black')
    
  draw_square(food[0], food[1], 9, 'green')
  update()
  Screen().ontimer(move, 100)

Screen().setup(420, 420, 370, 0)
hideturtle()
Screen().tracer(0, 0)

Screen().listen()
Screen().onkey(lambda: change(10, 0), 'Right')
Screen().onkey(lambda: change(-10, 0), 'Left')
Screen().onkey(lambda: change(0, 10), 'Up')
Screen().onkey(lambda: change(0, -10), 'Down')

move()

done()
```

A full listing is available here: [https://trinket.io/python/eed9b1f740](https://trinket.io/python/eed9b1f740)

## Challenges

To extend your program, try one of the following challenges:

* Don't move the snake until the player presses a direction.
* Write "Game Over" on the screen when the game finishes.
* Draw a box around the playing area so it is easier to see the edges.
* Make the snake go faster as it grows longer.
* Instead of the game finishing when you hit the edge of the screen, make the snake re-appear at the opposite edge of the screen.

