---
Title: Tron
category: python
order: 7
description: Tron game using the python language and turtle drawing library
---
In this activity, we will be creating a _Tron_ game, where two players control two light bikes
that leave a trail. If a player crosses the trail of the other player they lose and the game is over. 

**Taken and adapted from:**
[Free Python Games](http://www.grantjenks.com/docs/freegames/tron.html) under the [Apache 2 license](http://www.apache.org/licenses/LICENSE-2.0)

## 1-- Libraries and Variables

Go to <http://bit.ly/storm-coder-dojo-utilities> and use a project we've put a bit of code in already. Hit remix (log into your account if you have one or create one to save your progress). Then, in `main.py`:

-   Import all the classes from the `turtle` module into your project

```python
from turtle import *
```
Straight beneath this import the methods that we've already put in the project in `utilities.py`:
```python
from utilities import *
```
Beneath this put the variables that one of the light bikes will consist of

```python
p1xy = vector(-100, 0)
p1aim = vector(4, 0)
p1body = set()
```
`p1xy` is a vector object that has an `x` and a `y` value. It also has a `move` function that we'll use later. `p1aim` is another vector. It's used to aim `p1xy` when we move it. Finally, `p1body` is a set that will hold the information for the light trail that the bike leaves.

## 2-- Drawing

Define the function `draw`:
```python
def draw():
    p1xy.move(p1aim)
    p1head = p1xy.copy()

    p1body.add(p1head)

    square(p1xy.x, p1xy.y, 3, 'red')
    Screen().update()
    Screen().ontimer(draw, 50)
```
Here `p1xy` is moved towards the position of `p1aim`. Then `p1head` is made as a copy of `p1xy` and added to `p1body`. The body will be used later to check whether the bikes cross each other's trail. Then a red square is drawn at the point of `p1xy`. And finally the screen is updated.

Then `Screen().ontimer(draw, 50)` calls the draw method again every 50 milliseconds (every 1/20 of a second).

## 3-- Set the screen and start

At the bottom put the following:

```python
Screen().setup(420, 420, 370, 0)
hideturtle()
Screen().tracer(0,0)
draw()
done()
```

This sets up the screen, hides the turtle shape, stops the turtle from drawing until we call the update method and calls the `draw` function for the first time.

You should now be able to hit run, and see the red bike go right and off the screen.

## 4-- Add the second bike
Ok - over to you. Now you need to go over the code and add in the second bike. Look at the code and think about which bits you need to copy. If you're on a PC you'll find `ctrl c` to copy and `ctrl v` to paste really useful. On a Mac it's `cmd c` and `cmd v`.

## 5-- When bikes collide
Now that you've added the second bike it's time to put in logic to detect whether the bikes have hit each other's trail or gone off the screen.

Just after where you've set the variables define a function `inside` that takes a single parameter `head`:

```python
def inside(head):
    return -200 < head.x < 200 and -200 < head.y < 200
```
This returns true if the vector that's passed to it is greater than -200 or less than 200 in both the x and y axes. In other words – whether that vector is inside the screen.

After the code that moves the bikes and _before_ the code that adds `p1head` to `p1body` add in the following:
```python
    if not inside(p1head) or p1head in p2body:
        print('Player blue wins!')
        return
```
This checks whether the head is outside the screen or in the body of the other bike. If it is the other bike wins. Repeat this code for the other bike, but replace the values as appropriate.

If you do run now you should have the two bikes going towards each other until they crash. Blue will win because the check on the red bike is first in the code.

## 6-- steering the bikes
The last detail is to enable players to steer the bikes!

Near the bottom of the code after the `Screen().tracer(0,0)` add the following:
```python
Screen().listen()
Screen().onkey(lambda: p1aim.rotate(90), 'a')
Screen().onkey(lambda: p1aim.rotate(-90), 'd')
Screen().onkey(lambda: p2aim.rotate(90), 'j')
Screen().onkey(lambda: p2aim.rotate(-90), 'l')
```
This listens for key presses. And on the `a` and `d` key for player 1 rotates the aim left or right. `j` and `l` do the same for player 2.

The game is now complete!

## Full code listing
Your code should look something like this:
```python
from turtle import *
from utilities import *

p1xy = vector(-100, 0)
p1aim = vector(4, 0)
p1body = set()

p2xy = vector(100, 0)
p2aim = vector(-4, 0)
p2body = set()

def inside(head):
    return -200 < head.x < 200 and -200 < head.y < 200

def draw():
    p1xy.move(p1aim)
    p1head = p1xy.copy()

    p2xy.move(p2aim)
    p2head = p2xy.copy()

    if not inside(p1head) or p1head in p2body:
        print('Player blue wins!')
        return

    if not inside(p2head) or p2head in p1body:
        print('Player red wins!')
        return

    p1body.add(p1head)
    p2body.add(p2head)

    square(p1xy.x, p1xy.y, 3, 'red')
    square(p2xy.x, p2xy.y, 3, 'blue')
    update()
    Screen().ontimer(draw, 50)

Screen().setup(420, 420, 370, 0)
hideturtle()
Screen().tracer(0,0)
Screen().listen()
Screen().onkey(lambda: p1aim.rotate(90), 'a')
Screen().onkey(lambda: p1aim.rotate(-90), 'd')
Screen().onkey(lambda: p2aim.rotate(90), 'j')
Screen().onkey(lambda: p2aim.rotate(-90), 'l')
draw()
done()
```

## Challenges
To extend your program, try one of the following challenges:

* Don't start the game until a key is pressed
* Make the bikes go faster or slower
* Add a border to the edge of the screen
* Instead of the game finishing when you hit the edge of the screen, make the bikes re-appear at the opposite edge of the screen.

A full listing with challenges is available here: [http://bit.ly/TronChallenges](http://bit.ly/TronChallenges)
