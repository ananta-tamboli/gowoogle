---
layout: post
title: "07: Flow Control"
menutitle: "07: Flow Control"
date: 2019-07-01 19:35:00 +0000
tags: Python Tutorial Learning
category: [Python Tutorial, tutorial]
author: Ananta
image: assets/images/python-logo-master-v3-TM.png
---

# Learning Outcomes

- Understand how to use `if`, `else` and `elif`
- Construct for statements across an iterator
- Understand basics of for loop iteration in python
- Refresh Terminal skills

**NB** *As a reminder you should be using `ipython` now!*

## Contents

- This will become a table of contents (this text will be scraped).

# `if` statements

Perhaps the most well-known statement type is the if statement. For example:

```python
number = int(input('Input a number:'))
if number < 0:
    print("number is negative")
elif number % 2:
    print("number is positive and odd")        # Style Tip: Make sure you indent
else: print("number is even and non-negative") # this is legit python but it's UGLY - don't do this
```

There can be zero or more `elif` parts, and the `else` part is optional. The keyword `elif` is short for `else if`, and is useful to avoid excessive indentation. An `if` ... `elif` ... `elif` ... sequence is a substitute for the switch or case statements found in other languages.

# `for` Statements

The for statement in Python supports repeated execution of a statement or block of statements that is controlled by an iterable expression. Here’s the syntax for the for statement:

```python
# Measure some strings:
words = ['cat', 'window', 'floccinaucinihilipilification']
for w in words:
    print(w, len(w))
```

### Example: Loop until a condition Prime identification

Loop statements may have an else clause; it is executed when the loop terminates through **exhaustion of the iterable** (with `for`) or when the condition becomes `False` (with `while`), but **not** when the loop is terminated by a `break` statement.

```python
max_number = int(input('Enter a number to check primes up to:'))
for n in range(2, max_number):
    for x in range(2, n):
        if n % x == 0:
            print(n, 'equals', x, '*', n//x)
            break
    else: # only triggered if iterable is exhausted without breaking
        print(n, 'is a prime number')
```

### Example: Fibonacci Series

Here we use lists instead of the previous variable assignment method and run the iteration for precisely 5 loops rather than ending with a `while` condition

```python
result = [0, 1]
for i in range(5):
    result.append(result[-1] + result[-2])
```

which will then contain

```python
>>> result
[0, 1, 1, 2, 3, 5, 8]
```

# Exercises

## Exercise 7.1: Find all files matching pattern

The function `os.walk(source_directory)` will return three items `(root, directories, files)` at each iteration. There is one iteration for every end-point sub-directory in the directory tree

For example `os.walk('./root')` called from file `test.py`

```
test.py
root/
  dir1/
    f1.py
  dir2/
    f2.csv
    data.xlsx
    dir21/
  g.py
```

will iterate 4 times

- firstly, once at the `root` level returning `('./root', ['dir1', 'dir2'], ['g.py'])`
- once for `dir1` returning `('./root/dir1', [], ['f1.py'])`
- once for `dir2` returning `('./root/dir2', ['dir21'], ['f2.csv', 'data.xlsx'],)`
- finally, once for `dir21` returning `('./root/dir2', [], [],)`

**Note** that if you type `os.walk('./root')` it will return a `generator` object. To see the output of `generator` objects
you can just *generate* them by calling `list(the_object)` for for example here we would do `list(os.walk('./root'))`

### Exercise 7.1.1: Create the directory structure above using the Terminal commands we have learned in the first section

- **Hint** You will need `touch` / `mkdir`. `ls` and `mv` may also be helpful (On Windows just do `echo $null >> filename` instead of `touch`)

### Exercise 7.1.2: Print the path of all files that have an extension of `.py`

- **Hint** A modified form of the expression `if '.py' in 'files.py':` will be key
- **Hint** You can combine the `root` and the `filename` with `os.path.join`
- **Hint** Remember how to iterate an object that contains multiple items at each iteration. Multiple variable assignment! The following snippets might help you understand this

 ```python
 >>> for i, j, k in [['a0', ['b0', 'c0'], ['d0', 'e0']], ['a1', ['b1', 'c1'], ['d1', 'e1']]]:
 >>>     for l in k:
 >>>         print('second list elements', k)
 ```

```python
# Solve me!
```

You should get

```
./exercises/root/g.py
./exercises/root/dir1/f1.py
```

## Exercise 7.2: Searching and modifying file contents

In this exercise we look at a very common use of `python` which is a multi-file search and replace.

The following code snippet will edit some of the files you have created in 7.1. Look for common elements / repititions in this script and replace them with a loop

```python
import os
# os.path.join allows cross platform compatibility
# as windows uses \ and linux uses / to separate paths
# IT also allows you to combined and modify filepaths on the fly
base_dir = os.path.join('.', 'exercises', 'root','dir2')
filepath1 = os.path.join(base_dir, 'f2.csv')
filepath2 = os.path.join(base_dir, 'data.xlsx')
with open(filepath1, 'w') as fobj:  # WARNING! This line ERASES all contents in filepath1!
    fobj.write('test')              # if you want to read data ALWAYS use 'r'
with open(filepath2, 'w') as fobj:  # and do data = fobj.read() to explore the contents!
    fobj.write('test')
```

**Hint** File extensions e.g. `.xlsx` means nothing when programming. They are just hints to the operating system (Windows / OS X) of how to open the files when double clicking. I can even create a file like `myfile.alex.xlsx.rubbish`. Generally you will use common file extensions like `.txt` or `.dat` when dumping data.

### Exercise 7.2.1: Utility of context managers (the `with` statement)

This exercise is to show you why we tend to use the `with` statement (a context manager) when opening files.

Do some research on the line

```python
with open(filepath1, 'w') as fobj:
```

Try to understand what the difference between this and

```python
fobj = open(filepath1, 'w')
```

As an exercise, open the `.xlsx` file for reading in python using `f = open(filepath, 'r')` without closing the file object and now try opening it in Excel by double clicking in the explorer. Try again the other way round.

### Exercise 7.2.2: Finding files containing strings

Modify the solution to 7.1 to find all files containing the string `'test'`

**Hint** Don't open the files with `'w'` else you will overwrite with a blank file! The following may also be useful

```python
>>>'test' in 'test case this is a testcase'
True
```

### Exercise 7.2.3: Terminal recap: Copy directory contents as a backup

This exercise is a recap of the terminal skills in session 1. It is also extremely sensible when playing around with file contents as there is no `Cntrl+Z` once code is executed!

**In the Terminal** copy (not move!) the files from solution 7.2.2 into a new temporary directory `./exercises/backup_root/` as a backup

### Exercise 7.2.4: Find and replace

Find all files containing the string `'test'` and modify it to be `'test2'`. (This assumes you have ran the example above to create a file with `'test'` inside it). Check your solution.

**Hint** If you simply use `fobj.write('test2')` you will overwrite the entire file contents! This may not be ideal in most scenarios.

- Use the `open()` function this time like `open(filepath, 'r')` instead of `'w'`
- Then read the file contents to a temporary object
- Then open a new file protocol with `'w'` to **overwrite** the entire contents back to the file

# Next Topic

<!-- Wait until next week for "08: Functions and Editors" -->

[08: Functions and Editors](https://gowoogle.com/08-functions-and-editors/)