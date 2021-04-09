---
layout: post
title: "11: Classes and Modules"
menutitle: "11: Classes and Modules"
date: 2019-11-01 00:35:00 +0000
tags: Python Tutorial Learning
category: [Python Tutorial, tutorial]
author: Ananta
image: assets/images/python-logo-master-v3-TM.png
---

## Learning Outcomes

- Be able to create a basic class
- Interact with a class object
- Understand the utility of classes
- An idea of good and bad class usage
- Be able to create modular code
- Import from modular code

### Contents

- This will become a table of contents (this text will be scraped).

### Classes

Without necessarily knowing it you have been itnertacting with classes during this entire tutorial. That be said what is a class?

```python

class Dog:
    """
    Creates a Dog. Note that you will normally not document __init__ but instead
    put then documentation under the ``class`` definition
    
    Args:
        name: The dogs name
        color: give a color
        speed: Give the top speed in mph. Must be >= 0
        weight: Give the weight in kg. Must be >= 0
    
    Example:
        >>> Dog('rover', 'brown', 10, 50)
    """
    def __init__(self, name, color, max_speed, weight):
        self.name = name
        self.color = color
        self.max_speed = max_speed
        self.weight = weight
        
        if max_speed <= 0:
            raise ValueError('Cannot have max_speed <= 0')
        if weight <= 0:
            raise ValueError('Cannot have weight <= 0')

    def noise(self):
        """
        Makes the noise of a dog
        
        Example:
            >>> Dog('ronald', 'green', 10, 50).noise()
            'woof'
        """
        return 'woof'
```

A class is an abstraction of the logic that defines something. In this instance 'something' is a Dog - or rather a pretty basic dog. Currently all our dog can do is Woof!

The `__init__` method is used to define what arguments a class can take at it's creation. We can similarly define a class like

```python
class DogBase:

    def noise(self):
        return 'woof'
```

> **Style Tip** Always define classes in `ThisFormatOfText`

but the we cannot specify any defining characteristics about `DogBase` so it kind of loses it's individual charm - there are cases where `DogBase` would be increadibly useful but we won't touch on those at this level.

### What is `self` in a class

*Functions* within classes are known as *class methods*. This new namenclature kind of gives us a hint at a few things...

I can define a function outside of the class which doesn't overwrite the function inside the class

```python
>>> def noise()
...     return 'roar'
>>> Dog('ronald', 'purple', 10, 50).noise()
'woof'
```

### Why is it called `self`?

Secondly, notice that all my *class methods* contain the variable name `self` as the first argument whereas my *function* did not have this requirement.

> `self` is actually not required to be called `self`. In fact I could call it any name.
> It's the same as using `m` for mass in Newton's law `F = m * a`. I could easily replace Newton's
> law with `g = p * j` and state that `g` is force, `p` is mass and `j` is acceleration.

The reason we use `self` is because it's refers to the class object *itself*

### What does `self` do?

Lets create an instance of `Dog`

```python
>>> rover = Dog('rover', 'brown', 15, 50)
```

When we call

```python
>>> rover.noise()
```

what happens internally is actually

```python
>>> Dog.noise(rover)
```

thus `self` becomes `rover`!

#### Interacting with class attributes

Anything that is assigned to `self` becomes an attribute that we can interact with. This is another very useful property of a class: For storing intermediate results.

```python
class Demo:
    """
    A demo taking in 3 integers, x, y, z

    Example:
        >>> d = Demo(1, 2, 3)
        >>> (d.x == 1) & (d.y == 2) & (d.z == 3)
        True
    """
    def __init__(self, x, y, z):
        self.x = x
        self.y = y
        self.z = z

    def method(self, a, b):
        """
        Calculates ``(x^y + z) * a * b``

        Example:
            >>> x, y, z, a, b = 1, 5, 76, 89, 2
            >>> d = Demo(x, y, z)
            >>> d.method(a, b) - (x**y + z) * a * b  < 1e-6
            True

            We can also extract the temporary reuslt afterwards
            >>> d.tmp - (x**y + z) < 1e-6
            True

            and we also stored the result
            >>> d.res - (x**y + z) * a * b  < 1e-6
            True
        """
        self.tmp = self.x**self.y + self.z
        self.res = self.tmp * a * b
        return self.res
```

**Testing Tip** *As a side note I actually got the variable names on this really simple function
wrong several times simply because I decided to rename them all! THe doctests saved me actually putting up incorrect code.*

#### When to use Classes

This is all great but you don't work in a Game Studio. This is geared towards banking so why do we care about Dogs?! We don't, so lets start looking at trading strategies.

> Just as a heasd up: This has no relation to how Nomura QIS calculate momentum.
> This is just a simple demonstration to how we would want to utilise classes for signals

##### Start with ideas as snippets then functions

The best way to start is normally with functions or single lines of code. As you become comfortable
with components move then into functions.

Consider a simple momentum trading strategy: Simply define the momentum signal as the average of the sign of the return the `n` previous days

```python
import numpy as np

def momentum_signal(s):
    """
    A simple momentum signal
    
    Args:
        s: A numpy array of some underlying instrument price
    
    Example:
        We expect the returns to be calculated like
        
        >>> import numpy as np
        >>> s = np.array([1. , 1.5, 1. , 1.5])
        >>> rets_expected = np.array([1.5/1-1, 1/1.5-1, 1.5/1-1])
        
        Then we take the sign of these returns which gives
        
        >>> np.sign(rets_expected)
        array([ 1., -1.,  1.])
        
        Finally we average the sign like
        >>> expected = np.sign(rets_expected).sum() / 3.
        
        and we can compare against our function using `np.allclose`
        because sometimes the values may vary ever so slightly due to machine
        precision

        >>> result = momentum_signal(s)
        >>> np.allclose(result, expected)
        True
    """
    if not isinstance(s, np.ndarray):
        s = np.array(s)         # we need it to be a numpy array for efficiency
    rets = s[1:] / s[:-1] - 1   # See exercise 11.2 to understand this
    return np.sign(rets).mean()
```

This function is pretty generic bcause it will just give the signal on whatever prices you send into it. However, you need logic to take the last `n` prices from a larger timeseries and may want to do this in a rolling process so as to create a backtest.

These two conditions generate the need for a class

##### When to avoid classes

As a beginner classes will almost always make debugging more complicated if you dive into them head first.

Remember that abstraction is a tool: You should only use tools if they make your work more productive.

> An analogy is laying a drainage ditch in your back garden. An automated drainage ditcher
> is clearly far more adept at this task. By the time you source one and finally figure out how it works
> you could have already been at the pub. Also in using this new tool, it's likely
> that you may misconfigure it and have a lot of issues as a result! Pick the right tool for the right job!

###### Example: A bad class

I regularly see code from beginners that will define a single `class` in a module that will never
change in its parameterisation and will only ever be called once with functions that do indepedent tasks.

However, just because they functions were all for the specific task they may have wrote a class like

```python
from yourfirms_quantlib import core

class TradeThingyForReportX:
    """
    The report I was asked to automate yesterday for that client
    
    Args:
        my_trades: A list of trade id numbers

    Example:
        >>> report = TradeThingyForReportX([432346, 3546457, 345324])
        >>> report.my_trades
        [432346, 3546457, 345324]
    """
    def __init__(self, my_trades):
        self.my_trades = my_trades
   
    def value_trades_usd(self):
        """Loads trades from some central db and values them in USD
        
        Example:
            >>> report = TradeThingyForReportX([432346, 3546457, 345324])
            >>> report.value_trades_usd()
            2389456.34
        """
        return core.PresentValue(self.my_trades)
    
    def rebuild_forward_curves_with_new_data(self, new_data):
        """Loads trades from some central db and values them in USD

        Args:
            new_data: A list of some kind of new params for the trades

        Example:
            >>> report = TradeThingyForReportX([432346, 3546457, 345324])
            >>> report.rebuild_forward_curves_with_new_data([{'start_date': 1}])
            
            Lets assume we can check in the core quantlib if they are mutated
            >>> core.trade_start_date_shock([432346, 3546457, 345324])
            [1, 1, 1]
        """
        # ...
        # some manipulation to reformat new_data
        # ...

        # assume this changes the IDs in memory during this python session
        # althought this would probabaly be a terrible idea in reality!
        core.rebuild_forward_curves(my_trades, new_data)


    def get_present_value_with_new_data(new_data):
        """
        Gets the PV for a report X with some scenario modifications

        Args:
            new_data: A list of some kind of new params for the trades

        Example:
            Lets assume our new_data allows us to bump parameters then

            >>> report = TradeThingyForReportX([432346, 3546457, 345324])
            >>> report.get_present_value_with_new_data([{'start_date': 1}])
            92309.6
        """
        self.rebuild_forward_curves_with_new_data(new_data)
        return self.value_trades()
```

Given that this report will never be used outside of this junior's function: I would say that this is a **bad example of writing a class**.

###### Example: Avoiding bad classes

Instead I would write the above example as below which results in far less complexity and less code

```python
from yourfirms_quantlib import core

def rebuild_forward_curves_with_new_data(my_trades, new_data):
    """Loads trades from some central db and values them in USD

    Args:
        my_trades: A list of trade id numbers
        new_data: A list of some kind of new params for the trades

    Example:
        >>> rebuild_forward_curves_with_new_data([7878676], [{'end_date': -1}])

        Lets assume we can check in the core quantlib if they are mutated
        >>> core.trade_end_date_shock([7878676])
        [-1]
    """
    # ...
    # some manipulation to reformat new_data
    # ...

    # assume this changes the IDs in memory during this python session
    # althought this would probabaly be a terrible idea in reality!
    core.rebuild_forward_curves(my_trades, new_data)


def pv_for_report_x(my_trades, new_data):
    """
    Gets the PV for a report X with some scenario modifications
    
    Args:
        my_trades: A list of trade id numbers
        new_data: A list of some kind of new params for the trades
    
    Example:
        Lets assume our new_data allows us to bump parameters then

        >>> pv_for_report_x([432346, 3546457, 345324], [{'start_date': 1}])
        92309.6
    """
    rebuild_forward_curves_with_new_data(my_trades, new_data)
    return core.PresentValue(my_trades)
```

##### When to move into a class

In my opinion there are two good reasonas to use classes in `python`

1. You want to create many **parameterisations** of the **same underlying logic** (e.g. The `Dog` example)
2. You have many **functions** that are constantly **passing around the same objects**

##### A class for our backtesting

Lets create an engine that can give us the signal at any given historic timepoint for our momentum strategy

###### Use `pandas` for timeseries analysis

Doing timeseries analysis without using `pandas` is asking for trouble unless you're an advanced programmer.

This is because `pandas` contains an index which travels with the arrays that prevents you tying youeslf in knots. A single bad index can and will totally breakdown any systemmatic signal.

Therefore let us start the creation of our strategy using a pandas Series (a `pd.Series` is just a single column of a `pd.DataFrame`) check this by creating `df = pd.DataFrame({'a': [5, 6, 10]})` and selecting the column `'a'` like `df['a']`

###### Writing the base logic

The following incorporates the basic framework in which we want to operate

```python
class SimpleMom:
    """
    A simple momentum strategy
    
    Args:
        ts: The prices as a pandas DataFrame
        lookback: How many previous days we wish to create a signal from
    """
    def __init__(self, prices, lookback):
        if not isinstance(prices, pd.Series):
            raise ValueError('prices must be a pandas Series because we only want one column!')

        self.prices = pd.DataFrame(prices)
        self.lookback = lookback
```

Now lets add the signals in! This time we want to actually get the signal for a specific
historical time point so we need some kind of reference to the time in the dataframe index

```python
class SimpleMom:
    """
    A simple momentum strategy
    
    Args:
        ts: The prices as a pandas DataFrame
        lookback: How many previous days we wish to create a signal from

    Example:
        >>> import pandas as pd
        >>> prices = pd.Series([1. , 1.5, 1. , 1.5, 3.0])
        >>> sm = SimpleMom(prices, 3)
    """
    def __init__(self, prices, lookback):
        if not isinstance(prices, pd.Series):
            raise ValueError('prices must be a pandas Series because we only want one column!')

        self.prices = prices
        self.lookback = lookback

    def signal(self, t):
        """
        Gives the signal that we can observe at time ``t``
        
        Example:
            >>> sig = SimpleMom(pd.Series([1. , 1.5, 1. , 1.5, 4.0]), 3).signal(3)
            >>> np.allclose(1/3., sig)
            True
        """
        # this gives us all the prices up to but not including t
        # which is what we want because we can't observe the price at t
        # Think about EOD prices as an example... We can't see EOD for t
        # when are in day t
        prices_lookback = self.prices.loc[t - self.lookback: t]
        return momentum_signal(prices_lookback)
```

### Modular Python

At this stage I would **strongly recommend** turning on `autoreload` in `ipython` which can be done like

```ipython
In [0]: %load_ext autoreload

In [1]: %autoreload 2
```

This will allow your code to autoreload the changes as you edit them!

#### Example: Create a basic module

Create python files with the terminal in new directory called `nnet` the same directory like

```dir
./nnet/
    __init__.py
    core.py
```

This module will contain the generic mathematical building blocks
required for a neural network. Our main module `model.py` will bring all of these components together to make a model.

Inside `core.py` put the following contents.

```python
import numpy as np


def sigmoid(x):
    """
    The sigmoid function
    
    Example:
    
        This example shows how you can skip doctests
        on certain examples e.g. plots that are purely for
        demonstration and won't result in testable results

        >>> from nnet import core
        >>> import matplotlib.pyplot as plt  # doctest: skip
        >>> x = np.linspace(-10, 10, 100)    # doctest: skip
        >>> plt.plot(x, core.sigmoid(x))     # doctest: skip
        >>> plt.show()                       # doctest: skip
    """
    return 1 / (1 + np.exp(-x))

def forward_prop(x, w, b):
    """
    Forwards propagation
    
    Example:
        >>> import numpy as np
        >>> from nnet import core
        >>> x = np.array([[0],   # demonstrate that x is a column array
        ...               [1]])  # so has shape of (2, 1) - (rows, columns)
        >>> w = np.array([[.5, .5],
        ...               [.5, .5]])
        >>> b = np.array([[1], [1]])  # b is also a column array but on one line
        >>> core.forward_prop(x, w, b)
        array([[1.5],
               [1.5]])
    """
    # note that you can chain logic like below in python!
    if not (x.shape[0] == b.shape[0] == w.shape[-1]):
        raise ValueError('You have w, x, b wrong way round! '
                         f'Shapes: x: {x.shape}, w: {x.shape}, b: {b.shape}')
    return sigmoid(np.dot(w, x) + b)
```

`__init__.py` should be a blank file with no contents.

#### Example: Importing a Module

Now `cd` in `ipython` or the terminal so that `nnet` is a subdirectory. You will now be able to import the `nnet` module and use `core.sigmoid` as shown in the documentation example!

If you want to be able to import like

```python
>>> import nnet
>>> nnet.core.sigmoid(1)
0.5
```

you will also need to alter the `__init__.py` to the following

```python
from . import core
```

#### Example: Referencing other local modules

Create a new file `model.py` so that your directory looks like

```dir
./nnet/
    __init__.py
    core.py
    model.py
```

Inside `model.py` put the following

```python
import numpy as np

from . import core


def predict(x, w0, w1, b0, b1):
    """
    Returns the prediction of a neural network
    using specified weights and bias

    Example:
        >>> import numpy as np
        >>> from nnet import model
        >>> x0, x1 = 1, 0
        >>> x = np.array([[x0], [x1]])
        >>> w0 = np.array([[.5, .5], [.5, .5]])
        >>> w1 = np.array([[.5, .5], [.5, .5]])
        >>> b0 = np.array([[.25], [.25]])
        >>> b1 = np.array([[1], [1]])
        >>> model.predict(x, w0, w1, b0, b1).round(3)  # rounding avoids floating point issues
        array([[0.843],
               [0.843]])
    """
    assert all((isinstance(o, np.ndarray) for o in (x, w0, w1, b0, b1)))
    layer1 = core.forward_prop(x, w0, b0)
    return core.forward_prop(layer1, w1, b1)
```

**Style Tip** Leave to blank lines after imports. Leave one blank line between libraries installed by default (`import numpy as np` at the top) and your own created modules (`from . import core` below)

You will now similarly be able to import and call predict as in the example if your **present working directory** has `nnet` as a subdirectory.

You can check your present working directory by typing `pwd` into `ipython` and check to see if `nnet` is a subdirectory by doing `ls nnet` either in `ipython` or the terminal

*As an aside it should be slightly clearly why my bias for `ipython` is so strong*

#### Aside: How Python installs libraries

You have effectively created a mini-library here on-the-fly. This is infact not too far off how libs are
installed when you install a library in the terminal / powershell prompt like

```bash
python -m pip install SomeLibrary
```

What actually happens under-the-hood is that a system environment variable namd `PYTHONPATH` (similar to `PATH`) contains a list of all directories that `python` expects a python module (like you have created) to sit within as a subdirectory. These directories then behave as if your `ipython` terminal had them as the *present working directory* all at the same time.

As a hack you can see this working by doing

```python
>>> import sys
>>> sys.path.append('/directory/that/containsnnet')
```

Then even if you are not in the correct directory, you will still be able to import `nnet`. This can be quite useful during development. Just as a heads up - never put this in finished python modules as it is awefully hacky and a terrible idea in general!

## Exercises

### Exercise 11.1: Understand your `self`

Modify the `noise` so that the dog now speaks english. You should include these two `doctests` in the Examples
and make sure you can pass them

I have started this off for you as below. Note that you will need to rewrite the *class method* in `Dog`
with the below function so it will need to be indented and underneath the definition of `Dog`

```python
def noise():
    """
    Returns the noise of a fairly smart dog
    
    Example:
        >>> Dog('Rover', 'brown', 15, 50).noise()
        'I am Rover the Dog. I am brown. Woof!'
    
        >>> Dog('Sally', 'green', 15, 50).noise()
        'I am Sally the Dog. I am green. Woof!'
    """
    return

# Solve Me!
```

### Exercises 11.2: Fixing indexing errors

In the example where we create `SimpleMom` there is actually a major bug.

See what happens if you make the ``t`` smaller than the ``lookback``: In otherwords see what happens if you try and get a signal where there aren't enough historic prices available for the required ``lookback``

To further elaborate: If you have 4 prices in total as your history, you want to calculate the signal at day 2 but with a lookback window of 4 days... what happens!?

Propose a fix to this!

```python
# Solve Me!
```

## Next Topic

That's it that's all - Now go roam the wild ! At this stage you should be fairly proficient at beginner concepts. The best way to progress is to find a project idea to implement. Aim small with a very well defined success criteria. This is important because you want an end point at which you can determine you have succeeded and move on. It is also important to find something that you genuinely find interesting as a first project idea.

Migrating an Excel spreadsheet to python may seem useful but it is inantely boring as hell. How about controlling a toy drone? Scraping holiday websites for cheap deals? Silly fun ideas are by far the best ideas.

You are now truly a master of computers so go and have some realy fun!
