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
```
The first line sets up the screen size that we will be using to play the game.  We then 
hide the turtle so that we cannot see it while the screen is redrawing.  The `Screen().tracer(0, 0)` 
means that the drawing all happens in the background, and we will need to call `update()` to 
see any changes.  This will make our game run faster.


