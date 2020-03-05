---
Title: Microbit Python Activities 1
category: python
order: 11
description: Simple microbit activities in python
---
In this activity, we will create three programs for the Micro:bit, but we will use python instead of scratch.
The three activities are:
* A Step Counter
* A Movement Alarm
* A Compass Display

## Introduction

In your browser, go to the micro:bit website at [http://www.microbit.org](http://www.microbit.org) and follow the link to "Let's Code".
From here, go to the Python editor.

You should be presented with a simple "Hello World!" program

```python
# Add your Python code here. E.g.
from microbit import *


while True:
    display.scroll('Hello, World!')
    display.show(Image.HEART)
    sleep(2000)
```
Connect your micro:bit to your computer using the USB cable, and click the "Connect" toolbar option to download this program to the micro:bit.  You should see "Hello World!" scrolling across the LED screen.

## Step Counter

We will create a simple program that reads the accelerometer of the micro:bit.  Every time this exceeds a threshold, we will count this as a "step" and display our step count.

```python
from microbit import *
import math

count = 0
threshold = 1400 

while True:
    x = accelerometer.get_x()
    y = accelerometer.get_y()
    z = accelerometer.get_z()
    mag = math.sqrt(x*x +y*y +z*z)
    if(mag > threshold):
        count+=1
    display.show(count)
```

Try downloading your program to the micro:bit.  Attach a battery pack, then try walking round the room.  Does it count your steps?

The accelerometer may trigger too often, in which case you should add a `sleep(200)` to your program within the `while` loop.

### Challenges
* What happens if you change the threshold?
* What happens to the display if you take too many steps?  How could you show the full value of the count?

## Movement Alarm

In this activity we will create a simple movement alarm.  We will set the alarm using button A, and then trigger our alarm if the micro:bit is moved.

```python
from microbit import *

z = 0 # initialise z
alarm_triggered = False
thresh = 300
while True:
    if button_a.was_pressed():

        # Wait for 2 seconds while the alarm is armed
        display.show(Image.TRIANGLE)
        sleep(2000)
        z = accelerometer.get_z() # take a reading while still
        alarm_triggered = False   # the alarm is now armed

# Check to see if acceleration on z axis changed by threshold
    if accelerometer.get_z() < z - thresh:
        alarm_triggered = True
    if alarm_triggered:
        display.show(Image.NO)    # display a cross
    else:
        display.clear()
```

### Challenges
* How could we make the alarm less sensitive to movement?
* How could we turn the alarm off?


## Compass Display

In this activity we will use the micro:bit compass, and display the heading on the LED display.

First, we need to read the compass angle.  We create a `main` function to hold the main part of our program

```python
import math
from microbit import *

compass.calibrate()

# Functions go here


def main():
    
    while True:
        x = compass.get_x()
        y = compass.get_y()
        angle = math.atan2(y,x) * 180/math.pi
        print("x: ", x, " y: ", y, "angle: ", angle)
        sleep(500)

main()  
```

Now we need to convert this angle to one that we can use for a compass heading.  Add the following function in the code after the `compass.calibrate()`

```python
def convert_angle(theta):
    if (theta < 0):
        theta = theta+360
    elif (theta > 360):
        theta = theta - 360
    else:
        theta = theta
    return theta    
```

We now need to convert this to a compass heading.  Add the following function

```python
def angle_to_compass_heading(theta):
    if theta > 337.25 or theta < 22.5:
        return 'N'
    elif theta > 292.25 and theta < 337.25:
        return 'Nw'
    elif theta > 247.25 and theta <= 292.5:
        return 'W'
    elif theta > 202.5 and theta <= 247.5:
        return 'Sw'
    elif theta > 157.5 and theta <= 202.5:
        return 'S'    
    elif theta > 112.5 and theta <= 157.5:
        return 'Se'
    elif theta > 67.5 and theta <= 112.5:
        return 'E'
    else:
        return 'Ne'
```

Change your `main` function so that it looks like this:

```python
def main():
    
    while True:
        x = compass.get_x()
        y = compass.get_y()
        angle = math.atan2(y,x) *180/math.pi
        converted_angle = convert_angle(angle)
        heading = angle_to_compass_heading(converted_angle)
        display.show(heading)
        sleep(500)

```

