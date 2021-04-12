---
layout: post
title:  "Mad Libs Generator in python "
author: Ananta
date: "2017-10-07"
categories: [ mini project, tutorial, Python Tutorial ]
image: "assets/images/coding.jpg"
comments: false
---
Mad libs is a fun game that is usually played by kids.
In this python game player has to enter substitutes for blanks in the story without knowing the story. It will be fun to read aloud the stories after filling the blanks.

## Python Mad Libs Generator

The objective of this project is to develop a Mad Libs Generator python project. In this project, the user first has to pick a story by the title of the story. Then the user has to enter specific words like a noun, adverb, verb, food, adjective, etc, according to the requirements. And then the story will be generated.

This is a python project for absolute beginners and is developed using the basic concept of python and tkinter.

### Steps to develop Mad Libs Generator Game

These are the required steps to build Mad Libs generator python project:

1. Import modules
2. Create a display window
3. Define functions
4. Create buttons

### Project Prerequisites

Tkinter is a GUI Python library used to build GUI applications in the fastest and easiest way. To install the library, you can use the pip install command in command line:

```ssh
pip install tkinter
```

### Project File Structure

**1.** **Import Modules**

```python
from tkinter import *
```

The first step to build a project is to import the required modules. In this project, we import tkinter module.

**2.** **Create a Display Window**

```python
root = Tk()
root.geometry('300x300')
root.title('DataFlair-Mad Libs Generator')
Label(root, text= 'Mad Libs Generator \n Have Fun!' , font = 'arial 20 bold').pack()
Label(root, text = 'Click Any One :', font = 'arial 15 bold').place(x=40, y=80)
```

* **Tk()** initialized tkinter which means window created
* **geometry()** used when we want to set the width and height of the window
* **title()** used to give the title of the window
* **Label()** widget use to display text that users can’t able to modify
* **root** is the name which we give to our window
* **text** takes the text we want to display on the label
* **font** used to set the size and type of fonts
* **pack** organized widget in block
* **place()** used when we want to place widgets in a specific position

**3.** **Define Function**

```python
def madlib1():
    animals= input('enter a animal name : ')
    profession = input('enter a profession name: ')
    cloth = input('enter a piece of cloth name: ')
    things = input('enter a thing name: ')
    name= input('enter a name: ')
    place = input('enter a place name: ')
    verb = input('enter a verb in ing form: ')
    food = input('food name: ')
    print('say ' + food + ', the photographer said as the camera flashed! ' + name + ' and I had gone to ' + place +' to get our photos taken on my birthday. The first photo we really wanted was a picture of us dressed as ' + animals + ' pretending to be a ' + profession + '. when we saw the second photo, it was exactly what I wanted. We both looked like ' + things + ' wearing ' + cloth + ' and ' + verb + ' --exactly what I had in mind')


def madlib2():
    adjactive = input('enter adjective : ')
    color = input('enter a color name : ')
    thing = input('enter a thing name :')
    place = input('enter a place name : ')
    person= input('enter a person name : ')
    adjactive1 = input('enter a adjactive : ')
    insect= input('enter a insect name : ')
    food = input('enter a food name : ')
    verb = input('enter a verb name : ')

    print('Last night I dreamed I was a ' +adjactive+ ' butterfly with ' + color+ ' splocthes that looked like '+thing+ ' .I flew to ' + place+ ' with my bestfriend and '+person+ ' who was a '+adjactive1+ ' ' +insect +' .We ate some ' +food+ ' when we got there and then decided to '+verb+ ' and the dream ended when I said-- lets ' +verb+ '.')


def madlib3():
    person = input('enter person name: ')
    color = input('enter color : ')
    foods = input('enter food name : ')
    adjective = input('enter aa adjective name: ')
    thing = input('enter a thing name : ')
    place = input('enter place : ')
    verb = input('enter verb : ')
    adverb = input('enter adverb : ')
    food = input('enter food name: ')
    things = input('enter a thing name : ')
   
    print('Today we picked apple from '+person+ "'s Orchard. I had no idea there were so many different varieties of apples. I ate " +color+ ' apples straight off the tree that tested like '+foods+ '. Then there was a '+adjective+ ' apple that looked like a ' + thing + '.When our bag were full, we went on a free hay ride to '+place+ ' and back. It ended at a hay pile where we got to ' +verb+ ' ' +adverb+ '. I can hardly wait to get home and cook with the apples. We are going to make appple '+food+ ' and '+things+' pies!.')  
```

**4.** **Define Buttons**

Button(root, text= 'The Photographer', font ='arial 15', command= madlib1, bg = 'ghost white').place(x=60, y=120)
Button(root, text= 'apple and apple', font ='arial 15', command = madlib3 , bg = 'ghost white').place(x=70, y=180)
Button(root, text= 'The Butterfly', font ='arial 15', command = madlib2, bg = 'ghost white').place(x=80, y=240)

root.mainloop()

**Button()** widget used to display buttons on our tkinter window

command is called when the button is click and it is used to call the function

**root.mainloop()** used when we want to run our program

### Mad Libs Generator Game Output

![Alt](/ "alt")

### Summary

We have successfully developed the mad libs generator python project. We used tkinter library for rendering graphics on a display window and learn how to create buttons widget and pass the function to the button. In this way, we build this python project.

To Download Python Mad Libs Generator Code click the link below:

Source code of mad libs generator project: [Python Mad Libs Generator](https://gowoogle.com)
