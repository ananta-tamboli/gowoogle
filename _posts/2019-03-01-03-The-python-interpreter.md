---
layout: post
title: "03: The Python Interpreter"
menutitle: "03: The Python Interpreter"
date: 2019-03-01 22:32 +0000
tags: Python Tutorial Learning
category: [Python Tutorial, tutorial]
author: Ananta
image: assets/images/python-logo-master-v3-TM.png
---


### Learning Outcomes

- Able to launch / exit python in the terminal and run a simple command
- Able to import libraries
- Check python version
- Understand weakness of jit compilation

### Contents

- This will become a table of contents (this text will be scraped).

## Hello World

Open the `Terminal` and type

    python

This should then load the **basic** python interpreter. When we are in a python interpreter, I will use the following syntax: `>>>` for new logical blocks, `...` for line continuations or logical block continuations and results will be without padding.

Try entering the following to print thing to the console window

```python
>>> 'Hello World'
```

```output
'Hello World'
```

This gives the string representation of the string `'Hello World'` which is by definition `Hello World`

## Statements

A statement in `python` is something that is expressed in a single logical line such as

```python
>>> import sys
```

or another example is the `assert` statement which throws an error unless the following expression evaluates to `True`

```python
>>> assert True
```

You may be familiar with some other statements like

    - `break`
    - `continue`
    - `return`
    - `yield`
    - `raise`

amongst others which can be found on the [docs page](https://docs.python.org/3/reference/simple_stmts.html#grammar-token-expression-stmt)

## Using `python` libraries

To use python libraries we use the statement `import`

Worth noting is the following to check what `python` program is currently running your script (can be very important for debugging strange errors!)

```python
>>> import sys
>>> print(sys.executable)
/usr/local/anaconda3/bin/python
```

and for checking what version of `python` you have running. We should **all** be using at least python 3.6 by now (higher versions are fine)

```python
>>> print(sys.version)
3.6.5 |Anaconda, Inc.| (default, Apr 26 2018, 08:42:37)
[GCC 4.2.1 Compatible Clang 4.0.1 (tags/RELEASE_401/final)]
```

if you get `ModuleNotFoundError` this library is a non-standard `python` library and we can address that in another section.

## Exiting `python`

You can exit `python` by entering

```python
>>> exit()
```

If you are in the middle of executing code, you can force code to stop by entering `Control+C`. Sometimes you may have to hold `Control+C` or press it quite a few times to actually register particularly if you are doing something where the interpreter hasn't got time to register your command!

## Exercises

### Exercise 3.1: The issue with just-in-time (jit) complilation

This exercise is designed to demonstrate a common issue that can be caused by `python`'s JIT compilation (i.e. the fact that it runs one line at a time). In comparision `Fortran`, `java` & `c++` will check syntax across all the code (as well as numerous other checks) before you can run a single line.

In the `python` console type (this is meant to give a bug!)

```python
>>> print(HelloWorld)
```

Notice that the compiler tells you three things

- what file the issue was in
- what line the issue was on
- an arrow signalling which bit of code was bad
- An error type and description of the error

Lets write your first function (Note we will cover functions properly in section 8) as follows

```python
>>> def hello_world():
...     print(HelloWorld)
```

This function will execute everything indented until it hits a non-indented block (or a `return` / `yeild` statement). If this was `c++` we would instantly get an error as before.

However in `python` we won't get an error until the line itself is actually ran - similar to `VBA`

```python
>>> hello_world()
```

This is one of the biggest weaknesses of `python` and why most core quant libraries are still built in `java` or `C++`. Most of the bugs you create will be due to this.

**NB** You're not expected to fix this code yet! Just move onto the next exercise :)

## Next Topic

[04: Installing Python Packages](https://gowoogle.com/04-Installing-Python-Packages/)
