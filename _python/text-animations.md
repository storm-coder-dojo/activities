---
Title: Text animations
category: python
order: 4
description: Make text animations using the python console
---
In this activity, we will be making simple animations using text in
Python.

We will create a program that will draw some text, clear the screen,
make a change and draw the text again, and then repeat it. This creates
sequence of still images in quick enough succession that it looks like a
continuous moving image.

**Taken from:**
[101 Computing .net](https://www.101computing.net/text-based-animations/)

## 1 - Setup
Go to trinkiet.io and start a new Python project: <http://bit.ly/pythontrinket> (log into your account if you have one or create one to save your progress).

Import the `os` and `time` modules.
```python
import os
import time
```
The `os` module will allow us to issue commands directly to the text console where we will be showing the animations. The `time` module will allow us to make the program wait.

## 2 - Define the animation function
Write a function called `animate_rocket`:
```python
def animate_rocket():
```
Inside the function (remember it needs to be indented!) set a value for
how far from the top the rocket is going to start:
```python
distance_from_top = 20
```

## 4 - Draw the rocket
In line with the previous statement write a while loop that will
*always* run:
```python
while True:
```

Inside the `while` loop (remember it needs to be indented!) write the
series of print statements that will draw the rocket:
```python
    print("\n" * distance_from_top)
    print("          /\        ")
    print("          ||        ")
    print("          ||        ")
    print("         /||\        ")
```

The first line will print as many blank lines as we set above the while
loop. If we reduce the `distance_from_top` value we can move the rocket
upwards.

## 5 - Make changes
Still inside the `while` loop we'll make the rocket stay on the screen for
a short time then clear the text:
```python
time.sleep(0.2)
os.system('clear')
```

Now we'll make the rocket go in a constant loop from the bottom of the screen to the top:
```python
distance_from_top -= 1
if distance_from_top < 0:
    distance_from_top = 20
```

## 6 - Use the function
Let's blast off!

Outside of the function definition (no indentation, all the way to the left) call the function to launch the rocket!
```python
animate_rocket()
```

## 7 - Challenges
Draw some other animations, here are some ideas:

-   An animation that prints out a sentence letter by letter

-   An animation that mimics the rolling of a dice

-   An animation of a space invader

The solution for this activity can be found at:


## 8 - Solutions

### Rocket
<iframe src="https://trinket.io/embed/python/66ef07d632" width="100%" height="600" frameborder="0" marginwidth="0" marginheight="0" allowfullscreen></iframe>

### Sentence printer
<iframe src="https://trinket.io/embed/python/de1eefb2c1" width="100%" height="600" frameborder="0" marginwidth="0" marginheight="0" allowfullscreen></iframe>

### A die
<iframe src="https://trinket.io/embed/python/7170883b0b" width="100%" height="600" frameborder="0" marginwidth="0" marginheight="0" allowfullscreen></iframe>

### Space invader
<iframe src="https://trinket.io/embed/python/8727f1ba73" width="100%" height="600" frameborder="0" marginwidth="0" marginheight="0" allowfullscreen></iframe>
