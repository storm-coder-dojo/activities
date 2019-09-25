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

Go to <https://trinket.io/python/3f3e77a958> (Convert to bit.ly) to use a Trinket project that we have already created as a starter project.  Hit remix (log into your account if you have one or create one to save your progress). 

We have added some utility functions for you in `utilities.py` as well as some helpful starter code in `main.py`.

## 2. Draw the maze

The starter project already has an array called `tiles` that contains our maze.  In the array, a `0` represents a wall, and a `1` represents a path where we will add a dot for pac-man to eat.

Create a function called `world` after the `# Add your code here` comment.  Remember to be very careful about the indentation of each line - for example, make sure the `path.update()` is lined up exactly under the `for`.

```python
def world():
    screen.bgcolor('black')
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
    
    path.update()
```

This function sets the background of the screen to black, then uses the `path` turtle to draw a 20 pixel blue square for every tile with a value greater than zero.  If a tile value equals `1`, then we draw a white dot in the middle of the square.