---
layout: post
title: "02: Installing Python for Mac OS X / Windows"
menutitle: "02: Installing Python for Mac OS X / Windows"
date: 2019-02-01 22:32 +0000
tags: Python Tutorial Learning
category: [Python Tutorial, tutorial]
author: Ananta
image: assets/images/python-logo-master-v3-TM.png
---

### Learning Outcomes

- Know how to install python on your own machine
- Know how to find where python is currently installed

### Contents

- This will become a table of contents (this text will be scraped).

It's important not to skip steps. That is all.

We will use `anaconda` for simplicity. You can do what you like but I've installed `python` in lots of different ways on Linux, Mac and Windows and `anaconda` is by far the most straightforward for beginners.

## Windows

Make sure you **tick the bit that asks you if you want to add anaconda to your `PATH`** in the advanced settings. Otherwise you won't be able to *just run* `python` nicely from the `Terminal`.

Google something like *"How to install anaconda Windows"* and follow the guide.

## Mac OS X

Macs are a bit annoying in that you can't simply uninstall things nicely. To get around this we use `homewbrew` which is a *package manager* for Mac OS X.

Google something like *"installing hombrew mac os x"* and at the time of writing it should take you to <https://brew.sh/> with a line of code that you copy and paste into `Terminal`.

Normally sholdn't ever run code you don't understand but this code is pretty legit.

You must install `anaconda` via the `Terminal` and so we use `brew` to install `anaconda`.

Therefore, google something like *"install anaconda brew mac os x"* (Note: You **don't want the graphical installer!!**) I found [this link](https://stackoverflow.com/a/42505012/4013571) decent at the time of writing.

### Adding `python` to your `PATH` for Mac OS X

In Mac OS X you'll need to add the location of your `python` installation into a special file that is ran when the `Terminal` starts.

To do this we use a command to create a file if it doesn't exist

    touch ~/.bash_profile

The `~/` is a shortcut for your *root user file* or *home directory*. On Windows this is `C:\users\yourname`. The `.` at the beginning just denotes a hidden file.

We can then add the `PATH` to the python executable to this file by (please don't just copy this and run it - you are required to fill in the blanks!)

    echo 'export PATH=/path/to/your/conda/bin:"$PATH"' >>  ~/.bash_profile

Note that at the time of writing the `PATH` was `/usr/local/anaconda3/bin`. This command adds the line `export PATH=/path/to/your/conda/bin:"$PATH"` to the file `~/.bash_profile` by writing the output of the command `echo` to the file specified after the operator `>>` whcih signifies that you want to redirect the output and append to a file. For more information [see here](https://ss64.com/osx/syntax-redirection.html)

If you ever have weird things happen in the future when using `brew` check out <https://hashrocket.com/blog/posts/keep-anaconda-from-constricting-your-homebrew-installs>

## Exercises

### Exercise 2.1: Verifying your `python` executable

This exercise shows you how to debug a fairly common issue where users are running a different `python` executable to the one that they just installed.

Check what `python` is your system default

On Windows

    Get-Command python

On Mac OS X

    which python

### Exercise 2.2: Verifying your `python` version

This exercise helps you check your python version. Another source of confusion for some beginners.

Check you have python 3.6.5 or greater

    python --version

If you don't get `"Python 3.x.x :: Anaconda, Inc."` for `python` 3.6.5 or greater than seek help / reinstall.

### Exercise 2.3: Running a `.py` file in `python`

This exercise aims to show you how python files can be executed from the command line.

Run the file `~/pylrn/test.py` with `python`

## Next Topic

[03: The Python Interpreter](https://gowoogle.com/03-The-python-interpreter/)
