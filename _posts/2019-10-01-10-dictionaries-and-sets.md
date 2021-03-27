---
layout: post
title: "10: Dictionaries and Sets"
menutitle: "10: Dictionaries and Sets"
date: 2019-10-01 01:35:00 +0000
tags: Python Tutorial Learning
category: [Python Tutorial, Tutorial]
author: Ananta
image: assets/images/python-logo-master-v3-TM.png
---

# Learning Outcomes

- How to use dictionary / set objects and differences to list / tuple
- How to access dictionary objects in iteration
- Utility of dictionaries as hashmaps
- Utility of sets in dimensionality reduction

## Contents

- This will become a table of contents (this text will be scraped).

# Sets

Sets only contain unique items

```python
>>> trade_ids = [12342, 324562, 12342, 36452, 54767]
>>> set(trade_ids)
{12342, 36452, 54767, 324562}
```

We can also iterate a set like:

```python
>>> for i in trade_ids:
...     print(i, end=',')
12342,324562,12342,36452,54767,
```

## Set differences

They are also denoted by braces `{}`. Sets are a mathematical construct and `python` also supports some set logic such as set differences

```python
>>> trade_ids_expected = {12342, 36452, 54767, 324569}  # shorter way of defining sets
>>> unexpected_trade_ids = set(trade_ids) - trade_ids_expected
>>> unexpected_trade_ids
{324562}
```

we can also do it the other way round to look for missing trades

```python
>>> missing_trade_ids = trade_ids_expected - set(trade_ids)
>>> missing_trade_ids
{324569}
```

These two operations can be particularly useful when validating the inputs to functions.

## Sets items must be immutable

We can also iterate sets in the same way that we iterate lists and tuples. Objects can also be part of sets as long as they are immutable - i.e. unchanging. Recall that lists are mutable and tuples are immutable.

This means that we can have a set of tuples

```python
>>> set((1, 2,), (3, 4,))
{(1, 2,), (3, 4,)}
```

but not a set of lists

```python
>>> {[1, 2], [3, 4]}
---------------------------------------------------------------------------

TypeError                                 Traceback (most recent call last)

<ipython-input-2-a0ff115cb325> in <module>()
----> 1 {[1, 2], [3, 4]}


TypeError: unhashable type: 'list'
```

# Dictionaries

Dictionaries are python's version of that is known as a **hash map** or **hash table** in other languages. If anyone in an interview asks you for a hash map in `python` you'll know they just mean a `dict` (also they probably don't really know `python` that well!)

All this jargon means is a key-value lookup where the key is unambigouously unique. Think `VLOOKUP` in Excel but if there couldn't be any keys that are identical.

We set up a dictionary with a key value pair like follows

```python
>>> d = {
...     'akey': 'avalue',
...     'anotherkey': 'avalue'
... }
```

values can be anything.

I actually wrote a load of stuff about this but I deleted it because I think you shoudl get used looking at `python` documentation now you are more familiar with the language.

See the [offical python guide on `dict`](https://docs.python.org/3/tutorial/datastructures.html#dictionaries) - don't bother with dict comprehensions yet as we will come onto those but have a read of the dictionaries and looping techniques sections.

## Example: Trades by Asset Class

Lets assume we have the following data which we read in from a csv into a pandas DataFrame. Lets also assume that your credit and commodity desks for some reason give you the `trade_id` as a `str` - this is very annoying for you but a typical problem.

*Finally, for any more advanced readers this example is focused on `dict` and not `pandas` so we shall avoid using `pandas` for now*

```python
>>> import pandas as pd
>>> data = [['rates', 346455, 568789.345],
...         ['rates', 3467457, 4568679.345],
...         ['rates', 56858, -6578965789.45],
...         ['fx', 93875, 67896789.34],
...         ['fx', 34896, -3464754.456],
...         ['fx', 30986, 0.3456457],
...         ['credit', '234537', 45765.456],
...         ['credit', '457568', -3455436.213],
...         ['credit', '3467457', 456546.034],
...         ['commodities', '93875', -34563456.23235],
...         ['commodities', '34457', 4560456.4567],
...         ['commodities', '457478', 4575678.345346],
...         ['equities', 3466, -457567.345],
...         ['equities', 564756, -12.93045],
...         ['equities', 457568, 546636.438996]]
>>> df = pd.DataFrame(data, columns=['risk', 'trade_id', 'dv01'])
```

### How many trades are there per asset class with delta risk?

```python
>>> trade_by_asset_class = dict()
>>> for asset_class, trade_id in df.values:
...     trade_by_asset_class[asset_class] = trade_id
```

Lets now figure out what went wrong here... Remembering the stack method we see that there are too many values to unpack and that the arrow is on the `for` line (if you are useing pyCharm - you know who you are - then you may have no arrow!)

With iteration errors it is often easiest to index the first element to see why we couldn't unpack it:

```python
>>> df.values[0]
array(['rates', 346455, 568789.345], dtype=object)
```

Here we can see there are three items and we are trying to unpack to two elements `asset_class` and `trade_id` therefore we need a third element even if we don't currently care about the delta! A standard way of creating throwaway elements is to use `_` like

```python
>>> trade_by_asset_class = dict()
>>> for asset_class, trade_id, _ in df.values:
...     trade_by_asset_class[asset_class] = trade_id
```

but this doesn't really help because each iteration we have overwritten the value!

```python
>>> trade_by_asset_class
{'rates': 56858,
 'fx': 30986,
 'credit': '3467457',
 'commodities': '457478',
 'equities': 457568}
```

we therefore need to create a `list` as a value item and then append to the list - this is one of the most common dictionary structures.

```python
>>> trade_by_asset_class = dict()
>>> for asset_class, trade_id, _ in df.values:
...     if asset_class not in trade_by_asset_class:
...         trade_by_asset_class[asset_class] = []
...     trade_by_asset_class[asset_class].append(trade_id)
```

Think about these operations if you have a large number of rows: The following should bve quicker have a think about why this might be the case...

```python
>>> trade_by_asset_class = dict()
>>> for ac in set(df['risk']):
...     trade_by_asset_class[ac] = []
>>> for asset_class, trade_id, _ in df.values:
...     trade_by_asset_class[asset_class].append(trade_id)
```

which gives

```python
>>> trade_by_asset_class
{'rates': [346455, 3467457, 56858],
 'fx': [93875, 34896, 30986],
 'commodities': ['93875', '34457', '457478'],
 'equities': [3466, 564756, 457568],
 'credit': ['234537', '457568', '3467457']}
```

we now have a structure for answering the question:

```python
>>> for a, t in trade_by_asset_class.items():
...     print('risk: {:12s} trades: {:2d}'.format(a, len(t)))
risk: rates        trades:  3
risk: fx           trades:  3
risk: commodities  trades:  3
risk: equities     trades:  3
risk: credit       trades:  3
```

For more information string padding see: <https://pyformat.info/#string_pad_align>

### Simplifying iterations with dictionaries

Lets imagine that the credit trading PnL system for some reason prepends `'0s'` on all the database ids under a length of 7 because some lunatic decided it looked nice in the 90s.

To link your PnL you will have to also prepend zeros to every `trade_id`

_Whilst cussing out Diana Bloggs who retired last year after a distinguished trading career; but yet who also royally screwed you with one decision she made as a grad on a drizzly friday morning in 1999_

We can zero pad integers to a length of 7 like

```python
>>> str(346).zfill(8)
'00000346'
```

A naiive way of doing this would be just to call one of the following

```python
>>> df['trade_id_pad'] = df['trade_id'].astype(str).str.zfill(8)
```

> This example shows us two new operations: Firstly that we can call `.astype` on a
> `pandas.Series` (a series is a single column of a DataFrame).
> Secondly, if a `pandas` series is a `str` type then we can call `.str` to access
> operations that are normally found within `str` object types.

Imagine now that this dataframe is $10^6$x larger

> **WARNING if your laptop is aweful you may not want to run this next section**

```python
>>> df = pd.DataFrame(data*1e6, columns=['risk', 'trade_id', 'dv01'])
```

Timing this for me took about 10 seconds!

```python
In [100]: %timeit df['trade_id'].astype(str).str.zfill(8)
10.5 s ± 464 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```

Lets now simplify the process by using the hashmap

```python
In [101]: %%timeit
     ...: trade_ids = df['trade_id'].unique()  # pandas way to get unique items is fast
     ...: lookup = {}
     ...: for trade_id in trade_ids:
     ...:     lookup = {trade_id: str(trade_id).zfill(8)}
     ...: df['trade_id_pad'] = df['trade_id'].apply(lookup.get)
     ...: 
2.38 s ± 28.1 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```

> Here we see the `.apply` method in action. This is `pandas` version of a `map`.
> A map iterates a single function across an array of items.
> `map` actually exists in `python` as a default function and we can call it like:
>
> ```python
> >>> list(map(str, [3456, 4576, 7343]))
> ['3456', '4576', '7343']
> ```
>
> and
>
> ```python
> >>> list(map(len, ['36', '45sd76', '7343']))
> [2, 6, 4]
> ```
>
> `pandas.Series.apply` works in the same way and in this example iterates the
> `.get` method of `lookup` across every item in the dataframe.

Here the lookup method is exceedingly fast and creating it only requires us to use the far slower line `str(trade_id).zfill(8)` 15 times instead of 15 million times!

### Exercises

### Exercise 10.1: Dimensionality reduction

This example aims to build on previous examples to reinforce the idea of hash maps for reducing complexity.

You are working on an end-of-day regulatory risk model that requires the revaluation of all trades (e.g. [Basel III: FRTB Sensitivities Based Approach](https://www.bis.org/bcbs/publ/d352.pdf)).

You have been instructed to calculate the present value (PV) as a new column in an Excel sheet. Someone else has done this and complained it was impossibly long and took over 40 hours to calculate. They have requested access to a compute grid to speed up their Excel sheet worth $10k per year.

Assume your pricing function to get the PV of the trade is this:

```python
>>> import time
>>> import numpy as np
>>> np.random.seed(42)
>>> def my_pricing_function(trade_id):
...     """Gets the given a trade_id and returns a random pv"""
...     time.sleep(.1)
...     return 2e9 * np.random.random() - 1e9
```

and it is called in Excel something like `=MY_PRICING_FUNCTION($B3)` where `$B3` references the `trade_id` and is dragged down the column `B` for all 10000 rows.

Assume we have already read the Excel sheet with python and it gives us a dataframe like below

```python
>>> import pandas as pd
>>> data = [['rates', 346455, 568789.345],
...         ['rates', 3467457, 4568679.345],
...         ['rates', 56858, -6578965789.45],
...         ['fx', 93875, 67896789.34],
...         ['fx', 34896, -3464754.456],
...         ['fx', 30986, 0.3456457],
...         ['credit', '234537', 45765.456],
...         ['credit', '457568', -3455436.213],
...         ['credit', '3467457', 456546.034],
...         ['commodities', '93875', -34563456.23235],
...         ['commodities', '34457', 4560456.4567],
...         ['commodities', '457478', 4575678.345346],
...         ['equities', 3466, -457567.345],
...         ['equities', 564756, -12.93045],
...         ['equities', 457568, 546636.438996]]
>>> df = pd.DataFrame(data * 10000, columns=['risk', 'trade_id', 'dv01'])
```

Currently this pricing function is being called like

```python
>>> df['pv'] = df['trade_id'].apply(my_pricing_function)
```

Use your knowledge of dictionaries to reduce the problem set and claim a portion of the cost savings for your bonus.

```python
# Solve me
```

<!-- ## Exercise 10.2: Extracting corporate financials via an API
This example aims to help you understand how to navigate a simple dictionary object and also that there is a library for almost every problem. *You should note that in a real Investment Banking use case there will be an API for internal / paid datasets and I would recommend you talk to your IT / Data desk / Strat emphasising that every dataset available in Excel will be available in `python`*

I googled `"python get balance sheet"` and found the following decent library: https://github.com/JECSand/yahoofinancials 
    
We can tell this is a decent library because:
  - It has good documentation
  - There are issues being worked upon [recently](https://github.com/JECSand/yahoofinancials/issues/41)
  - The last commit was recent
These are all extremely important particularly when the core premise of the library is webscraping a site that can (and does regularly!) change.

We can follow the example on the [github homepage](https://github.com/JECSand/yahoofinancials#lists-of-tickers-example) and adapt for our use case.

I found for various reasons I needed to do a few improvements to this API to get exactly what we needed. We can `pip install` libraries directly from github. Please **use the improved-fork I created of this API** until it is accepted by the library author as follows:

```bash
$ pip install git+https://github.com/flipdazed/yahoofinancials
```

Your exercise is to use this API to extract the Equity for Apple. Lets assume that Apple is a 100% public company (I have no idea if it is or not) so then the equity is just the Market Cap.

**Hint** The ticker for Apple is `"AAPL"`... How did I know market capitalisation was definitely there? Check the API documentation!

```python
# Solve me
```

## Exercise 10.3: Comparable (Comp) analysis : Calculating Levered Beta
This is example aims to demonstrate some use cases of IBD-style analysis that can be done in python *NB: I work in trading and not IBD so apologies for any mistakes!*

The $\beta$ is the a measure used in CAPM modelling to measure the systemmatic risk of an individual stock vs. the unsystemmatic risk of the entire market (c.f. https://www.investopedia.com/terms/b/beta.asp)

In order to calculate the $\beta$ from comps we must first "deleverage" the $\beta$ for each comp which can be done via

$$
\beta_{unlev}\left(1 + (1-r_T)\frac{D}{E}\right) = \beta
$$

where $D$ is debt, $E$ is equity, $r_T$ is effective tax rate and $E_p$ is preferred stock.

As a toy example lets calculate the $\beta$ of Microsoft (MSFT) from some comparables and compare to the Yahoo value.

Lets take the [main competitors](https://www.investopedia.com/ask/answers/120314/who-are-microsofts-msft-main-competitors.asp) and use them as comps in the examples that follow 

```python
>>> comps = ['AAPL', 'SAP', 'IBM', 'ORCL', 'GOOG']
```

## Exercise 10.3.1: Obtain the Debt/Equity Ratios for comps
This example shows you a real result of an API which typically contain nested dictionary objects and how they can be parsed into a `pandas.DataFrame` object for ease of use.

You can create a `pandas.DataFrame` object like follows:

```python
>>> import pandas as pd
>>> data = {'AAPL': {'grossMargins': 0.346, 'profitMargins': 45.23},
...         'SAP': {'grossMargins': 346, 'profitMargins': 23}}
>>> df_comps = pd.DataFrame(data)
>>> df_comps
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>AAPL</th>
      <th>SAP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>grossMargins</th>
      <td>0.346</td>
      <td>346</td>
    </tr>
    <tr>
      <th>profitMargins</th>
      <td>45.230</td>
      <td>23</td>
    </tr>
  </tbody>
</table>

I can assure you that it will be a bit simpler if we transpose this dataframe like follows and use the tickers as the row indices as well like follows. This will let us use columns for our calculations. As beginners in `pandas` objects I think this easier

```python
>>> df_comps = pd.DataFrame(data).T
>>> df_comps
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>grossMargins</th>
      <th>profitMargins</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>AAPL</th>
      <td>0.346</td>
      <td>45.23</td>
    </tr>
    <tr>
      <th>SAP</th>
      <td>346.000</td>
      <td>23.00</td>
    </tr>
  </tbody>
</table>

To start you off, this will get the data - you  can find this all on the `README.md` of the github repository
```python
>>> from yahoofinancials import YahooFinancials
>>> comps = ['AAPL', 'SAP', 'IBM', 'ORCL', 'GOOG']
>>> parser = YahooFinancials(comps)
>>> bals = parser.get_financial_data()  # This is why we use my fork: We can't get D/E from the original
```

Convert this to a `pandas` DataFrame as shown above where the tickers are the row indices. Then filter the columns to only leave only the column `['debtToEquity']` this can be done by
```python
>>> df_comps = df_comps[['debtToEquity']]
>>> df_comps['debtToEquity'] /= 100.  # we need to convert the values from percentages to decimal
```

```python
# Solve me
```

## Exercise 10.3.2: Calculate the Effective Tax Rate for Comps
This example will contain a non-trivial data structure containing lists and dictionaries nested within each other. It will also challenge you interact with the `pandas.DataFrame` object you have just created using `.loc` to create new entries

Calculate the effective tax rate for all these tech companies: This is given in the API as `incomeTaxExpense / incomeBeforeTax` as in https://github.com/JECSand/yahoofinancials#examples-of-returned-json-data

I will start you off with the following

```python
>>> inc = parser.get_financial_stmts('annual', 'income')  # annual is more stable estimate to avoid rebates
```

**Hint** that you can create a new column/row combination in a dataframe by doing:

```python
>>> df_comps.loc['AAPL', 'incomeBeforeTax'] = 45
```
You will want to iterate through each ticker and column combination to fill the values

```python
# Solve me
```

## Exercise 10.3.3: Calculate the beta for each comp
This example requires you to read the technical documentation for a repository and utilise it for a solution.

Create a new column for the $\beta$ for each comp.

Explore the documentation at https://github.com/JECSand/yahoofinancials to find a sensible method (There is more than one way to do this!)

```python
# Solve me
```

## Exercise 10.3.4: Calculate unlevered beta as a new column
This example uses some basic vectorised mathematics with pandas DataFrames

If you have forgotten the formula to calculate $\beta_{unlev}$ is

$$
\beta_{unlev} = \frac{\beta}{1 + (1-r_T)\frac{D}{E}}
$$

Calculate the $\beta_{unlev}$ for each comp in a new column called `'beta_unlev'`. You should google how to do basic maths in `pandas` if you are unsure.

```python
# Solve me
```

## Exercise 10.3.5: Calculate the beta for Microsoft
We now need to relever the $\beta$ with the debt-equity ratio and effective tax rate of microsoft.

### Exercise 10.3.5: Obtain $D/E$ and Effective Tax rate for Microsoft
This exercise revises the previous dictionary and list circus skills

Repeat just the vital steps above to obtain `'debtToEquity'` and `effectiveTax` for `"MSFT"`

```python
# Solve me
```

### Exercise 10.3.5: Calculate the estimate of unlevered beta as the median of the comps
The exercise demonstrates how we can use `numpy` functionality through `pandas` 

This is a maths problem and so naturally we use `numpy` which has the function `np.median`. We can use this on the column of the `pd.DataFrame` containing the `'beta_unlev'` to obtain the median, remembering that Pandas DataFrames are really just wrappers around numpy arrays

> **Note** Most simple math functions can be found as a `.method()` on the end of the pandas DataFrame.
> For example try typing
> 
> ```python
> >>> df_comps['beta_unlev'].
> ```
> Then hit TAB after the `.` and see all the methods available. You should find one to calculate the median too.

Assign the median beta to a variable name `beta_unlev_msft`

```python
# Solve me
```

### Exercise 10.3.5: Calculate the estimate of beta
The calculate $\beta$ for Microsoft as
$$
\beta_{unlev}\left(1 + (1-r_T)\frac{D}{E}\right) = \beta
$$
where $r_T$ is the effective tax rate for Microsoft and D/E is the Debt-to-Equity ratio for Microsoft and we already calcualted $\beta_{unlev}$ above

```python
# Solve me
```

# Appendix: Calculate WACC for Microsoft from comparables
This exercise is really aimed at those in Investment Banking roles because it's nice to complete the picture

## Calculate beta based upon comparables D/E

As a guess of an "optimal" capital structure from comparables, take the median debt and median equity from the comparables. Calculate the D/E ratio from these medians and use it to create a $\beta$, using the same `effectixeTax`. Add a new row containing `['debtToEquity', 'effectiveTax', 'beta']` for this new $\beta$ and D/E using the index name: `'MSFT_beta_optcap'`

> This won't affect Bankers working in the insudstry as you'll have your own systems / APIs firms...
> However, sometimes the `'totalDebt'` can be a bit rogue in Yahoo Finance. I would recommend pulling
> (check API documentation!) the `'totalShareholderEquity'` as the total Equity and calculating `'totalDebt'`
> from the trivial relation
>
> ```python
total_debt = debt_to_equity * total_shareholder_equity 
> ```

So we don't give away the solutions to previous sections, I will hardcode the expected results required (as of November 13th!) below and use that in this demonstration

```python
>>> df_msft = pd.DataFrame(
...     [[0.8068, 0.101813, 1.229776, 0.7130553],
...      [0.8068, 0.101813, 1.073342, 0.6223507]],
...     columns=['debtToEquity', 'effectiveTax', 'beta', 'beta_unlev'],
...     index=['MSFT_beta_hist', 'MSFT_beta_comp']
... )
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>debtToEquity</th>
      <th>effectiveTax</th>
      <th>beta</th>
      <th>beta_unlev</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>MSFT_beta_hist</th>
      <td>0.8068</td>
      <td>0.101813</td>
      <td>1.229776</td>
      <td>0.713055</td>
    </tr>
    <tr>
      <th>MSFT_beta_comp</th>
      <td>0.8068</td>
      <td>0.101813</td>
      <td>1.073342</td>
      <td>0.622351</td>
    </tr>
  </tbody>
</table>

and the comparables
```python
>>> df_comps = pd.DataFrame(
...     [[1.194, 65.737e9, 10.481e9, 0.1594, 1.247, 0.6224, 90.488e9, 108.047e9, 0.4558, 0.5442],
...      [0.5439, 5.6e9, 1.511e9, 0.2698, 1.0666, 0.7634, 30.159e9, 16.403e9, 0.6477, 0.3523],
...      [3.9512, 11.342e9, 2.619e9, 0.2309, 1.3442, 0.3328, 17.956e9, 70.947e9, 0.202, 0.798],
...      [2.9852, 12.268e9, 1.185e9, 0.0966, 1.1506, 0.3112, 18.433e9, 55.027e9, 0.2509, 0.7491],
...      [0.0763, 34.913e9, 4.177e9, 0.1196, 1.0146, 0.9508, 194.969e9, 14.872e9, 0.9291, 0.0709]],
...      index=[ 'AAPL', 'SAP', 'IBM', 'ORCL', 'GOOG'],
...      columns=['debtToEquity', 'incomeBeforeTax', 'incomeTaxExpense',
...               'effectiveTax', 'beta', 'beta_unlev', 'totalStockholderEquity',
...               'totalDebt', 'ratioEquity', 'ratioDebt']
... )
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>debtToEquity</th>
      <th>incomeBeforeTax</th>
      <th>incomeTaxExpense</th>
      <th>effectiveTax</th>
      <th>beta</th>
      <th>beta_unlev</th>
      <th>totalStockholderEquity</th>
      <th>totalDebt</th>
      <th>ratioEquity</th>
      <th>ratioDebt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>AAPL</th>
      <td>1.1940</td>
      <td>6.573700e+10</td>
      <td>1.048100e+10</td>
      <td>0.1594</td>
      <td>1.2470</td>
      <td>0.6224</td>
      <td>9.048800e+10</td>
      <td>1.080470e+11</td>
      <td>0.4558</td>
      <td>0.5442</td>
    </tr>
    <tr>
      <th>SAP</th>
      <td>0.5439</td>
      <td>5.600000e+09</td>
      <td>1.511000e+09</td>
      <td>0.2698</td>
      <td>1.0666</td>
      <td>0.7634</td>
      <td>3.015900e+10</td>
      <td>1.640300e+10</td>
      <td>0.6477</td>
      <td>0.3523</td>
    </tr>
    <tr>
      <th>IBM</th>
      <td>3.9512</td>
      <td>1.134200e+10</td>
      <td>2.619000e+09</td>
      <td>0.2309</td>
      <td>1.3442</td>
      <td>0.3328</td>
      <td>1.795600e+10</td>
      <td>7.094700e+10</td>
      <td>0.2020</td>
      <td>0.7980</td>
    </tr>
    <tr>
      <th>ORCL</th>
      <td>2.9852</td>
      <td>1.226800e+10</td>
      <td>1.185000e+09</td>
      <td>0.0966</td>
      <td>1.1506</td>
      <td>0.3112</td>
      <td>1.843300e+10</td>
      <td>5.502700e+10</td>
      <td>0.2509</td>
      <td>0.7491</td>
    </tr>
    <tr>
      <th>GOOG</th>
      <td>0.0763</td>
      <td>3.491300e+10</td>
      <td>4.177000e+09</td>
      <td>0.1196</td>
      <td>1.0146</td>
      <td>0.9508</td>
      <td>1.949690e+11</td>
      <td>1.487200e+10</td>
      <td>0.9291</td>
      <td>0.0709</td>
    </tr>
  </tbody>
</table>

### Obtain Total Debt and Total Equity for comps
I use the last `'quarterly`' data for unlevering $\beta$
```python
>>> from yahoofinancials import YahooFinancials
>>> comps = ['AAPL', 'SAP', 'IBM', 'ORCL', 'GOOG']
>>> parser = YahooFinancials(comps)
>>> bal_comps = parser.get_financial_stmts('quarterly', 'balance')
```

Then extract the relevant elements from the balance sheet API return values and calculate the `'totalDebt'`
and `totalEquity` ratios

```python
>>> for p in comps:
...     bal_last = list(bal_comps['balanceSheetHistoryQuarterly'][p][0].values())[0]
...     df_comps.loc[p, 'totalStockholderEquity'] = bal_last['totalStockholderEquity']
>>> df_comps['totalDebt'] = df_comps['debtToEquity'] * df_comps['totalStockholderEquity']
>>> df_comps['ratioEquity'] = df_comps['totalStockholderEquity'] / (df_comps['totalDebt'] + df_comps['totalStockholderEquity'])
>>> df_comps['ratioDebt'] = 1 - df_comps['ratioEquity'] # A lot of assumptions here!
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>debtToEquity</th>
      <th>incomeBeforeTax</th>
      <th>incomeTaxExpense</th>
      <th>effectiveTax</th>
      <th>beta</th>
      <th>beta_unlev</th>
      <th>totalStockholderEquity</th>
      <th>totalDebt</th>
      <th>ratioEquity</th>
      <th>ratioDebt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>AAPL</th>
      <td>1.1940</td>
      <td>6.573700e+10</td>
      <td>1.048100e+10</td>
      <td>0.1594</td>
      <td>1.2470</td>
      <td>0.6224</td>
      <td>9.048800e+10</td>
      <td>1.080427e+11</td>
      <td>0.455789</td>
      <td>0.544211</td>
    </tr>
    <tr>
      <th>SAP</th>
      <td>0.5439</td>
      <td>5.600000e+09</td>
      <td>1.511000e+09</td>
      <td>0.2698</td>
      <td>1.0666</td>
      <td>0.7634</td>
      <td>3.015900e+10</td>
      <td>1.640348e+10</td>
      <td>0.647710</td>
      <td>0.352290</td>
    </tr>
    <tr>
      <th>IBM</th>
      <td>3.9512</td>
      <td>1.134200e+10</td>
      <td>2.619000e+09</td>
      <td>0.2309</td>
      <td>1.3442</td>
      <td>0.3328</td>
      <td>1.795600e+10</td>
      <td>7.094775e+10</td>
      <td>0.201971</td>
      <td>0.798029</td>
    </tr>
    <tr>
      <th>ORCL</th>
      <td>2.9852</td>
      <td>1.226800e+10</td>
      <td>1.185000e+09</td>
      <td>0.0966</td>
      <td>1.1506</td>
      <td>0.3112</td>
      <td>1.843300e+10</td>
      <td>5.502619e+10</td>
      <td>0.250928</td>
      <td>0.749072</td>
    </tr>
    <tr>
      <th>GOOG</th>
      <td>0.0763</td>
      <td>3.491300e+10</td>
      <td>4.177000e+09</td>
      <td>0.1196</td>
      <td>1.0146</td>
      <td>0.9508</td>
      <td>1.949690e+11</td>
      <td>1.487613e+10</td>
      <td>0.929109</td>
      <td>0.070891</td>
    </tr>
  </tbody>
</table>

### Obtain Total Debt and Total Equity for Microsoft
We can also extract the `'totalStockholderEquity'` for Microsoft as well

```python
>>> ticker = "MSFT"
>>> parser = YahooFinancials(ticker)
>>> bal_msft = parser.get_financial_stmts('quarterly', 'balance')
>>> bal_msft = list(bal_msft['balanceSheetHistoryQuarterly'][ticker][0].values())[0]
>>> df_msft.loc[:, 'totalStockholderEquity'] = bal_msft['totalStockholderEquity']
>>> df_msft['totalDebt'] = df_msft['debtToEquity'] * df_msft['totalStockholderEquity']
>>> df_msft['ratioEquity'] = df_msft['totalStockholderEquity'] / (df_msft['totalDebt'] + df_msft['totalStockholderEquity'])
>>> df_msft['ratioDebt'] = 1 - df_msft['ratioEquity']
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>debtToEquity</th>
      <th>effectiveTax</th>
      <th>beta</th>
      <th>beta_unlev</th>
      <th>totalStockholderEquity</th>
      <th>totalDebt</th>
      <th>ratioEquity</th>
      <th>ratioDebt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>MSFT_beta_hist</th>
      <td>0.8068</td>
      <td>0.101813</td>
      <td>1.229776</td>
      <td>0.713055</td>
      <td>106061000000</td>
      <td>8.557001e+10</td>
      <td>0.553465</td>
      <td>0.446535</td>
    </tr>
    <tr>
      <th>MSFT_beta_comp</th>
      <td>0.8068</td>
      <td>0.101813</td>
      <td>1.073342</td>
      <td>0.622351</td>
      <td>106061000000</td>
      <td>8.557001e+10</td>
      <td>0.553465</td>
      <td>0.446535</td>
    </tr>
  </tbody>
</table>

### Include an 'Optimal' capital structured based on comps
Create new 'optimal' capital structure by taking the medians of the comps Debt and Equity ratios

```python
>>> i = f'{ticker}_beta_optcap'
>>> df_msft.loc[i, :] = df_msft.loc[f'{ticker}_beta_hist', :].copy()
>>> df_msft.loc[i, 'ratioDebt'] = df_comps['ratioDebt'].median()
>>> df_msft.loc[i, 'ratioEquity'] = df_comps['ratioEquity'].median()
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>debtToEquity</th>
      <th>effectiveTax</th>
      <th>beta</th>
      <th>beta_unlev</th>
      <th>totalStockholderEquity</th>
      <th>totalDebt</th>
      <th>ratioEquity</th>
      <th>ratioDebt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>MSFT_beta_hist</th>
      <td>0.8068</td>
      <td>0.101813</td>
      <td>1.229776</td>
      <td>0.713055</td>
      <td>1.060610e+11</td>
      <td>8.557001e+10</td>
      <td>0.553465</td>
      <td>0.446535</td>
    </tr>
    <tr>
      <th>MSFT_beta_comp</th>
      <td>0.8068</td>
      <td>0.101813</td>
      <td>1.073342</td>
      <td>0.622351</td>
      <td>1.060610e+11</td>
      <td>8.557001e+10</td>
      <td>0.553465</td>
      <td>0.446535</td>
    </tr>
    <tr>
      <th>MSFT_beta_optcap</th>
      <td>0.8068</td>
      <td>0.101813</td>
      <td>1.229776</td>
      <td>0.713055</td>
      <td>1.060610e+11</td>
      <td>8.557001e+10</td>
      <td>0.455789</td>
      <td>0.544211</td>
    </tr>
  </tbody>
</table>

### Create beta values for the various capital structures

```python
>>> df_msft.loc[i, 'totalDebt'] = df_msft.loc[i, 'ratioDebt'] * (df_msft.loc[f'{ticker}_beta_hist', 'totalDebt'] + df_msft.loc[f'{ticker}_beta_hist', 'totalStockholderEquity'])
>>> df_msft.loc[i, 'totalStockholderEquity'] = df_msft.loc[i, 'ratioEquity'] * (df_msft.loc[f'{ticker}_beta_hist', 'totalDebt'] + df_msft.loc[f'{ticker}_beta_hist', 'totalStockholderEquity'])
```

```python
>>> df_msft.loc[i, 'debtToEquity'] = df_msft.loc[i, 'totalDebt'] / df_msft.loc[i, 'totalStockholderEquity']
>>> df_msft.loc[i, 'beta'] = df_msft.loc[i, 'beta_unlev'] * (1 + (1 - df_msft.loc[i, 'effectiveTax']) *  df_msft.loc[i, 'debtToEquity'])
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>debtToEquity</th>
      <th>effectiveTax</th>
      <th>beta</th>
      <th>beta_unlev</th>
      <th>totalStockholderEquity</th>
      <th>totalDebt</th>
      <th>ratioEquity</th>
      <th>ratioDebt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>MSFT_beta_hist</th>
      <td>0.8068</td>
      <td>0.101813</td>
      <td>1.229776</td>
      <td>0.713055</td>
      <td>1.060610e+11</td>
      <td>8.557001e+10</td>
      <td>0.553465</td>
      <td>0.446535</td>
    </tr>
    <tr>
      <th>MSFT_beta_comp</th>
      <td>0.8068</td>
      <td>0.101813</td>
      <td>1.073342</td>
      <td>0.622351</td>
      <td>1.060610e+11</td>
      <td>8.557001e+10</td>
      <td>0.553465</td>
      <td>0.446535</td>
    </tr>
    <tr>
      <th>MSFT_beta_optcap</th>
      <td>1.1940</td>
      <td>0.101813</td>
      <td>1.477761</td>
      <td>0.713055</td>
      <td>8.734322e+10</td>
      <td>1.042878e+11</td>
      <td>0.455789</td>
      <td>0.544211</td>
    </tr>
  </tbody>
</table>

## Calculate Cost of Equity, ${r}_{E}$
Lets assume the following below

- implied equity risk premium is as of 1st Nov: http://pages.stern.nyu.edu/~adamodar/
- risk free rate taken from 10 year US Treasury bonds when writing on 13th Nov
- Current cost of debt, ${r}_{D}$, for Microsoft

```python
>>> erp = 0.0509
>>> rf = 0.0189
>>> debt_cost_msft = 0.033539
```

Then cost of equity can be calculated as

```python
>>> df_msft['costDebt'] = debt_cost_msft
>>> df_msft['costEquity'] = rf + erp * df_msft['beta']  # cost_equity = rf + erp * beta_levered 
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>debtToEquity</th>
      <th>effectiveTax</th>
      <th>beta</th>
      <th>beta_unlev</th>
      <th>totalStockholderEquity</th>
      <th>totalDebt</th>
      <th>ratioEquity</th>
      <th>ratioDebt</th>
      <th>costDebt</th>
      <th>costEquity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>MSFT_beta_hist</th>
      <td>0.8068</td>
      <td>0.101813</td>
      <td>1.229776</td>
      <td>0.713055</td>
      <td>1.060610e+11</td>
      <td>8.557001e+10</td>
      <td>0.553465</td>
      <td>0.446535</td>
      <td>0.033539</td>
      <td>0.081496</td>
    </tr>
    <tr>
      <th>MSFT_beta_comp</th>
      <td>0.8068</td>
      <td>0.101813</td>
      <td>1.073342</td>
      <td>0.622351</td>
      <td>1.060610e+11</td>
      <td>8.557001e+10</td>
      <td>0.553465</td>
      <td>0.446535</td>
      <td>0.033539</td>
      <td>0.073533</td>
    </tr>
    <tr>
      <th>MSFT_beta_optcap</th>
      <td>1.1940</td>
      <td>0.101813</td>
      <td>1.477761</td>
      <td>0.713055</td>
      <td>8.734322e+10</td>
      <td>1.042878e+11</td>
      <td>0.455789</td>
      <td>0.544211</td>
      <td>0.033539</td>
      <td>0.094118</td>
    </tr>
  </tbody>
</table>

## Calculate the Weighted Average Cost of Capital (WACC)
The WACC formula is (ignoring tweaks and preferred stock)

$$
{WACC} = {r}_{E} \frac{E}{D+E} + {r}_{D} \frac{D}{D+E}(1 - r_{eff})
$$

so we can calculate in `python` as

```python
>>> df_msft['WACC'] = df_msft['costEquity'] * df_msft['ratioEquity'] + df_msft['costDebt'] * df_msft['ratioDebt'] * (1 - df_msft['effectiveTax'])
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>debtToEquity</th>
      <th>effectiveTax</th>
      <th>beta</th>
      <th>beta_unlev</th>
      <th>totalStockholderEquity</th>
      <th>totalDebt</th>
      <th>ratioEquity</th>
      <th>ratioDebt</th>
      <th>costDebt</th>
      <th>costEquity</th>
      <th>WACC</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>MSFT_beta_hist</th>
      <td>0.8068</td>
      <td>0.101813</td>
      <td>1.229776</td>
      <td>0.713055</td>
      <td>1.060610e+11</td>
      <td>8.557001e+10</td>
      <td>0.553465</td>
      <td>0.446535</td>
      <td>0.033539</td>
      <td>0.081496</td>
      <td>0.058556</td>
    </tr>
    <tr>
      <th>MSFT_beta_comp</th>
      <td>0.8068</td>
      <td>0.101813</td>
      <td>1.073342</td>
      <td>0.622351</td>
      <td>1.060610e+11</td>
      <td>8.557001e+10</td>
      <td>0.553465</td>
      <td>0.446535</td>
      <td>0.033539</td>
      <td>0.073533</td>
      <td>0.054150</td>
    </tr>
    <tr>
      <th>MSFT_beta_optcap</th>
      <td>1.1940</td>
      <td>0.101813</td>
      <td>1.477761</td>
      <td>0.713055</td>
      <td>8.734322e+10</td>
      <td>1.042878e+11</td>
      <td>0.455789</td>
      <td>0.544211</td>
      <td>0.033539</td>
      <td>0.094118</td>
      <td>0.059292</td>
    </tr>
  </tbody>
</table>

It turns out that this is slightly off the mark with what I found on the internet but I suspect that is due to the $\beta$ on Yahoo Finance being paticularly high compared to other sources

 -->

# Next Topic

<!-- Wait until next week for "11: Classes and Modules" -->

[11: Classes and Modules](https://gowoogle.com/11-classes-and-modules/)