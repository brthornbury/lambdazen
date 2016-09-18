### A Better Lambda
[![PyPI version](https://badge.fury.io/py/lambdazen.svg)](https://badge.fury.io/py/lambdazen)
[![Build Status](https://travis-ci.org/brthornbury/lambdazen.svg?branch=master)](https://travis-ci.org/brthornbury/lambdazen)

**What is this?**

A better python lambda syntax for your anonymous function needs. 

Write `a = (x) > x` instead of `a = lambda x: x`. See below for syntax caveats.

Get started immediately: `pip install lambdazen`

```python
from lambdazen import zen
def otherfunc(*args):
    print sum(args)

# The zen decorator allows you to define lambdas with a better syntax
@zen
def example():
    example.epic = (x, y, z) > otherfunc(x, y, z)

    # Multiline lambdas are a tuple or list of statements
    # The assignment operator inside is << instead of =
    # The last statement is the return value
    example.multiline = (x, y, z) > (
        s << otherfunc(x, y, z),
        s
    )

# Call function so the lambdas are bound to function attributes
example()

example.epic(1,2,3)
>>> 6

example.multiline(1,2,3)
>>> 6
```

**Caveats**
 - better lambdas can only be defined in a function with the `@zen` attribute
 - any other code in this function will be executed, it's best to use the function as a container of lambdas

**How does it work**

[Read the story](https://github.com/brthornbury/lambdazen/blob/master/HowItWorks.md)

TLDR; Runtime in-memory source rewriting and recompilation

**Additional Examples**

```python
from lambdazen import zen

# Lambdas don't need to be bound to the function
@zen
def normalizeString(nS):
    transforms = [
        (s) > s.strip(),
        (s) > s.lower(),
        (s) > s.replace(' ', '_')]

    apply_all = (transforms_list, s) > (
        is_done << (len(transforms_list) == 0),
        transforms_list[0](apply_all(transforms_list[1:], s)) if not is_done else s)

    return apply_all(transforms, nS)

normalizeString("Abraham Lincoln")
>>> "abraham_lincoln"
```
