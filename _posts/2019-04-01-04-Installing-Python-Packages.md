---
layout: post
title: "04: Installing Python Packages"
menutitle: "04: Installing Python Packages"
date: 2019-04-01 22:32 +0000
tags: Python Tutorial Learning
category: [Python Tutorial, Tutorial]
author: Ananta
---

## Learning Outcomes

- Be able to install libraries from pypi using `pip`
- Be able to install libraries requiring C++ compilers using `conda`
- Understand that machines require precision in commands
- Be able to locate a proxy in a corporate setting

## Contents

- This will become a table of contents (this text will be scraped).

Run the following

```sh
python -c "import pyfolio"
```

we will see how to fix this issue

# `pip`

`pip` is the default `python` package manager. It is to `python` what `homebrew` is to Mac OS X.

Install a new `PACKAGE` with

```sh
python -m pip install PACKAGE
```

for example

```sh
python -m pip install pyfolio --proxy https://path.to.my.proxy:1234 -IU
```

Here we introduce three very useful flags when in a corporate environment. Almost all corporations will have proxies. You need to specify the proxy to hit the internet programmatically.

To see other flags type

```sh
python -m pip install -h
```

# `conda`

Recall `anaconda` is what we installed `python` with. This is a more sophisticated python package manager.

Some libraries like `scipy` require advanced C++ libraries (LAPACK / BLAS) in the case of `scipy`. These can be **extremely** difficult to install on Windows machines in particular *(last time installing LAPACK/BLAS from scratch took me 2 days reading documentation plus 8 hours of compilation!)*

`conda` comes with prebuilt versions and compilers which mean you get scipy in under a minute

```sh
conda install scipy
```

Downsides are that not all libraries are supported by `conda` and there is less help on Stackoverflow for it.

# Exercises

## Exercise 4.1: Supplying arguments to a program

This exercise is to help you get used to the idea of self help in coding.

Try and figure out what the `-IU` means and notice that you can stack single letter flags on a single `-`. The combination of `-IU` is usually only used when an installation is broken or doing something weird and won't work.

## Exercise 4.2: Debugging bad command

I can't really tell you the aim of this exercise without giving away the answer :)

Why does this not work?

```sh
python - m pip -h
```

**Hint** It shouldn't just take you to the `python` console. It should show you the Help for `pip`!

## Exercise 4.3: Understanding importance of `$PATH`

This exercise aims to educate you at how your machine works under the hood. How does your machine know how to execute programs in `Program Files` or other obscure locations? This should make that clear.

Recall that the first command must be an executable program. Why does `$ pip -h` work?

**Hint** On Windows powershell type `$ $env:PATH` or on Mac OS X type `$ echo $PATH`.
You can use `ls` to display files that match patterns, for example

```sh
ls "/a/directory/*pip*"
```

use this on the `/bin` directory for anaconda.

## Exercise 4.4: Corporate Proxies

This exercise is really more of an example to make a point that most people in corporate environments think IT have *blocked them* from using `python` - genuinely some of the smartest quants I know have also thought this. When in fact it's nothing of the sort. Instead it's just a symptom of being behind a firewall.

If you are on a corporate network find your proxy.

**NB: If you are working in a bank (e.g. Nomura) please check internally what the correct proxy is to use - doing the below is NOT the correct thing to do if there is a specific PyPi server internally (Nomura employees see confluence page on this)**

If you do not have any specific internal instructions then the following is how to find a Corporate Proxy: In windows follow [this guide](https://superuser.com/a/346376). On Mac OS X follow [this guide](https://askubuntu.com/a/924676)

This proxy is hardcoded in your browser e.g. Google Chrome / IE and is how it hits the internet. The example above shows how to `pip install` using a proxy.

# Next Topic

[05: Basic Mathematical Operations & Debugging](https://flipdazed.github.io/blog/python%20tutorial/05-Basic-Mathematical-Operations-and-Debugging)
