---
layout: post
title: "08: Functions and Editors"
menutitle: "08: Functions and Editors"
date: 2019-08-01 19:35:00 +0000
tags: Python Tutorial Learning
category: [Python Tutorial, Tutorial]
author: Ananta
image: assets/images/python-logo-master-v3-TM.png
---

# Learning Outcomes

- Declare a function with good style
- Variable scope
- Include examples and use as tests with `doctest`
- Writing code in a text editor / IDE

## Contents

- This will become a table of contents (this text will be scraped).

# Defining functions

- The keyword `def` introduces a function *definition*
- follow with a single space
- name of function (**style tip** always name functions like `lower_case_with_underscores`)
- parenthethis containing comma separated arguments

The statements that form the body of the function start at the next line, and must be indented.

```python
def raise_to_power(a, b):
    """prints the results of ``a**b``"""
    result = a**b
    print(result)

result = raise_to_power(4, 3)
```

If the last line of a function finishes without `return` then the returned value is the python object: `None` - we can check this by using the built-in function `type`

```python
>>> type(result)
NoneType
```

We can instead `return` the value as below: Note that we no longer get a printed value, intead we get an `Out` value

```python
def raise_to_power(a, b):
    """returns the results of ``a**b``"""
    return a**b

raise_to_power(6, 2)
```

functions themselves are an object

```python
>>> raise_to_power
<function __main__.raise_to_power(a, b)>
```

We can therefore assign and override functions that have been declared in the same way as we can do for variables, for example

```python
raise_to_power_2 = raise_to_power
raise_to_power_2(6, 2)
```

and then we can overwrite it with. This isn't normally sensible but shows how easy it is to checnge values in `python`

```python
raise_to_power = 2
raise_to_power
```

## Docstrings

For smaller shorter functions it's acceptable to write a simple one line description.
For longer less obvious functions you should write a more descriptive docstring

```python
def example_function(arg1, arg2, keywordarg1='keywordvalue1'):
    """
    This is a doc string describing the function.
    Generally use 3 double quotes for docstrings
    
    Args:
        arg1: the first argument
        arg2: the second argument
        keywordarg1: a keyword argument
    
    Example:
        >>> example_function(1, 2, 3)
        (1, 2, 3)
    """
    return arg1, arg2, keywordarg1
```

Google-style python docstrings are a common way to write documentation in python. Furthermore, advanced users can [automatically generate](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/) static webpage documentation from docstrings. Two good examples of this are

- [`requests`](https://realpython.com/python-requests/)
- [`numpy`](https://numpy.org/doc/1.17/reference/arrays.ndarray.html)

The following is a really good reference for the syntax but the above is a good minimal example

<https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html>

### Doctests

The most important part of this docstring is the example. Note the syntax: It is a deliberate replication of what we would see in the python console.

It can be used as a test case to ensure the function runs as expected as follows

```python
import doctest
doctest.testmod()
```

**Note** The result `(1, 2, 3)` must be **exactly** how the result is in the return value. For example

```python
>>> example_function(1, 2, 3)
(1, 2, 3,)
```

will fail the test result as it will simply compare text results not `python`. It is easy to create these examples by just running small snippets in `python`/`ipython` and pasting the result in the Example.

Lets not get too hung up on this now but if we do this correctly we can all the following for a small time investment:

- A unit test like below
- An use example
- static documentation

In my personal opinion the first two are invaluable particularly in Investment Banking and Trading where someone may need to understand / run / change your models.

A full `doctest` reference can be found here: <https://docs.python.org/2/library/doctest.html>

## Variable Scope

The execution of a function introduces a new symbol table used for the local variables of the function. This  can be seen in the following example

```python
a = 100
d = "d is a Global Variable"


def demo_locals(a, b):
    """Prints ``a, b, c, d`` where c is external to function"""
    d = 'An Example of a Local Variable taking preference'
    print(a, b, c, d)

# Using global variables like this in functions
# is a sure fire way to screw yourself over as a beginner
c = 200
    
demo_locals(1, 2)
print(d)
```

    1 2 200 An Example of a Local Variable taking preference
    d is a Global Variable

Under the local (function) scope, the global variable `d` is overwritten with a new local value. The following `print` statement shows that this override only persists in the local scope within the function declaration. Finally, we can use global variables from within the function (e.g. ``c``) providing they are in the global variable scope before the function is executed.

### Scope Summary

- Functions introduce a new local symbol table
- Functions revert to look up varible names in the global table as second precedence
- Local **variable** declarations / overrides do not persist globally
- If a function calls a second function (e.g. `print`) the new local scope is also declared for that function
- Defining a function is like defining a variable. It becomes present in the current scope.

Note the emphasis on variable. We shall see examples later where we can modify global states from within functions. There are specific ways to set global variables from functions but in general it's a terrible idea so we won't even cover it.

# Using a Text Editor / IDE

Now we will write longer more complex code we shoudl use a text editor / integrated development environment (IDE). These both present a sane way of editing longer code.

## Text Editor or IDE? What's the difference

A text decent editor is simply a tool that will highlight the lines of code you write and possibly *lint* it for you. Linting is the act of checking for basic errors. Some may also offer autocompletion options. Often they will allow multiple plugins that are each individually aimed at increasing your productivity in different areas.

An IDE however, is typically a fixed set of addins with a specific look-and-feel. In fact really an IDE is just a text editor on steriods and once you start adding lots of additional functionality to your text editor it basically turns into an IDE.

I'll only bother introducing 2 options to keep it simple (and for my own sanity)

- `PyCharm` as an IDE
- `SublimeText3` as a Text Editor
- `Jupyter` as an IDE

Choosing your text editor / IDE is a fairly lifelong commitment so like marriage you will be stuck in it for better or for worse; for richer or for poorer. You may get into arguments with other people about their editors. Occasionally people cheat on their first editor but this is an unusual move.

I would recommend trying a few editors before making this commitment. Like most people though, you will probably settle for the first one in front of you.

**If you have already chosen your editor then you can skip the rest of this section and move to the exercises as the rest is me opining on why I use `SublimeText`**

## My Setup / Why I hate IDEs and use a Text Editor

Ok now this is entirely my own opinion but I don't really like IDEs: Especially for beginners...

I personally use `SublimeText3` and `ipython` to write code. I test out snippets in `ipython` and adjust them in `SublimeText` as required. I run sections of code in my text editor by copying to my clipboard and running `%paste` in `ipython`

### Additional configuration

More often than not, IDEs require a certain level of set up because of their additional functionality. Taking pyCharm as an example, if you set up your project settings incorrectly it can be extremely hard for a beginner to ascertain if your error is a `PyCharm` config error or an actual issue with your code. A more explicit example: If you configure you IDE to use a different `python` installation by mistake, none the libraries  you thought should be installed will be installed. This can really throw you off as a newbie.

### Obsfucation

IDEs intentionally obsfucate or abstract away some areas which we have already covered like Terminal skills and knowledge of `PATH` evnironment variables. This can often mean that new users think of `python` as being something that is tied to their IDE. For example, a new user can often think of programs in a Windows sense (e.g. Microsoft Office): Microsoft Word does not exist outside the program that opens that big white blank sheet on your machine. However `python` can run fine without your IDE.

I personally thought for a month that `Fortran` code (my first language) could only be written in a special program. It took a while for me to realise that all the text editor does is produce a file with words in it.

### Sensory Overload

You're already learning a new progeamming language. Learning a new editor just adds insult to injury when you have a bug in your code and also get confuse with your IDE.

Text editors allow you to build your own custom IDE as your skill level increases, slowly adding highly customised optionality for your own style.

# Exercises

## Exercise 8.1: Abstracting with functions

This example will help you to identify possible variable values in small snippets of code to generalise them.

Take Geometric Brownian Motion as defined below
$S_{t+1} = S_t e^{(r - \frac{1}{2}\sigma^2) \Delta t + \sigma\epsilon\sqrt{\Delta t}}$
and write it as function generating a random variable with `numpy` using a random seed of `42`.

I have started it off for you below

```python
def gbm(s, r=0.01, sigma=0.1, dt=1/252):
    pass
```

Note that `pass` is used as above for placeholder functions that contain nothing. You will of course need to actually return a value. Finally, we also introduce default **keyword arguments** - writing them like above we can assign default values.

**Style Tip** Don't put any spaces in default keyword argument definitions

```python
# Solve Me!
```

```python
assert gbm(100, .02, .1, 1/252) - 100.31936176261115 < 1e-10
```

## Exercise 8.2: Doctests

The following minimal example is a way of calling the function above and guaranting the same result

```python
>>> np.random.seed(42)
>>> s = gbm(100, .02, .1, 1/252)
>>> s - 100.31936176261115 < 1e-10
True
```

Use this example to write a proper docstring for the function `gbm` and run the test case with the `doctest` module as shown further up the page in an earlier example.

A good way of running the test cases in `ipython` is below

```python
import doctest
doctest.run_docstring_examples(the_function_you_want_to_test, globals(), True)
```

`globals()` is a function that contains all the global variables currently defined (in your ipython session). It might be worth searching the internet for the difference between `globals()` and `locals()` to further get an idea of how functions are used in python!

## Exercise 8.3: The `XOR` function

Given two variables `a` and `b` the `OR` function will return `True` for every instance except when

```python
>>> a = False
>>> b = False
>>> a or b
False
```

The Exclusive OR function (`XOR`), however, will also return `False` when

```python
>>> a = True
>>> b = True
False
```

write a function for this in python taking in two varibles `a` and `b` as arguments. Write the four test cases in the doctstring in `doctest` format and test that the function works as expected by running `doctest`

```python
# Solve Me!
```

## Exercise 8.4: Functions called from functions

Let us introduce a new method of doing loops for this example:

```python
for i in range(10):
    print(i)
```

Recall the stock motion example in Exercise 5.2: Write a function to calculate 10 steps of Geometric Brownian Motion. Call the function `gbm` from within this function.

Use the same default values as before in Exercise 5.2 and don't forget to set the random seed as below so you can compare your solution!

```python
import numpy as np
np.random.seed(42)


# Solve Me!
```

The following will help you check your code

```python
assert gbms(100, 4, 0.02, .1, 1/252) - 101.4864244028311 < 1e10
```

## Exercise 8.5: Recursion

This is not really an exercise but it is worth being aware of as recursion is a typical interview question.

You can also call a function itself from within itself! As an example the Fibonacci Series can be written

```python
def fibonacci(n):
    """Returns the nth number of the fibonacci series

    Args:
        n: An positive integer

    Example:
        We can compare to a theoretical value

        >>> z = (5**0.5 + 1) / 2
        >>> theoretical = int((z**10 - (-z)**-10) * 5**-0.5)
        >>> fibonacci(10) == theoretical
        True
    """
    if n < 0 or not isinstance(n, int):
        raise ValueError('`n` must be a positive integer')
    elif n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fibonacci(n - 1) + fibonacci(n - 2)
```

The factorial function `n!` is defined as the product of all numbers 1 to `n` (including `n`) i.e.

$$
n! \equiv \prod_{x=0}^n x = 1 \times 2 \times \dots \times (n - 2) \times (n - 1) \times n
$$

where the null product is defined as equal to 1.

Explicitly

- $0! = 1$
- $1! = 1$
- $5! = 1\times2\times3\times4\times5 = 120$.

# Next Topic

[09: Lists and Tuples](https://flipdazed.github.io/blog/python%20tutorial/09-lists-and-tuples)
