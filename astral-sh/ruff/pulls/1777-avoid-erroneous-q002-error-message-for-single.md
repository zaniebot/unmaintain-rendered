```yaml
number: 1777
title: Avoid erroneous Q002 error message for single-quote docstrings
type: pull_request
state: merged
author: colin99d
labels: []
assignees: []
merged: true
base: main
head: fix_q002
created_at: 2023-01-11T03:08:02Z
updated_at: 2023-01-12T01:21:50Z
url: https://github.com/astral-sh/ruff/pull/1777
synced_at: 2026-01-12T05:36:32Z
```

# Avoid erroneous Q002 error message for single-quote docstrings

---

_Pull request opened by @colin99d on 2023-01-11 03:08_

Fixes #1775. Before implementing your solution I thought of a slightly simpler one. However, it will let this function pass:
```
def double_inside_single(a):
    'Double inside "single "'
```
If we want function to pass, my implementation works. But if we do not, then I can go with how you suggested I implemented this (I left how I would begin to handle it commented out). The bottom of the flake8-quotes documentation seems to suggest that this should pass:
https://pypi.org/project/flake8-quotes/


---

_Comment by @colin99d on 2023-01-11 03:12_

I went to check how flake8-quotes handles this, and it looks like out of the box they throw an error on what we are trying to fix.
'''
def double_inside_single(a):
    "This is a test"
'''
Results in:
resources/test/fixtures/flake8_quotes/docstring_doubles_function.py:31:5: Q002 Single quote docstring found but double quotes preferred

So I will leave it up to you to decide on the quotes inside quotes edge case I found above. After that I will add the test fix and we can merge this thing!

---

_Comment by @charliermarsh on 2023-01-11 04:11_

Okay I'm struggling to understand the originating plugin's logic but here's my guess.

If you have `Double` enabled, these should be ok:

```py
def f():
    """Docstring"""

def f():
    "Docstring"

def f():
    """Doc'string"""

def f():
    "Doc'string"

def f():
    'Doc"string'

def f():
    '''Doc"string'''
```

And these should error:

```py
def f():
    '''Docstring'''

def f():
    'Docstring'
```

If you have `Single` enabled, the reverse should be true. So all of these should be ok:

```py
def f():
    '''Docstring'''

def f():
    'Docstring'

def f():
    '''Doc"string'''

def f():
    'Doc"string'

def f():
    "Doc'string"

def f():
    """Doc'string"""
```

---

_Comment by @charliermarsh on 2023-01-11 04:12_

(I think we probably have the same bug as the library, because I copied the logic exactly.)

---

_Comment by @colin99d on 2023-01-11 12:12_

At first I made a complicated function:
```
fn contains_good_docstring(quote: &Quote, raw_str: &str) -> bool {
    match quote {
        Quote::Single => raw_str.contains("'''") || raw_str.contains("'"),
        Quote::Double => raw_str.contains("\"\"\"") || raw_str.contains('"'),
    }
}
```
But now im realizing we simplify it to this:
```
fn contains_good_docstring(quote: &Quote, raw_str: &str) -> bool {
    match quote {
        Quote::Single => raw_str.contains("'"),
        Quote::Double => raw_str.contains('"'),
    }
}
```

And with this we can pretty much just take it back to the original function. But with a single character instead of triples.

---

_Comment by @colin99d on 2023-01-11 12:16_

Changes in! This should work as "if single there needs to be a single quote SOMEWHERE, and if double there needs to be a double quote SOMEWHERE".

---

_Renamed from "Fix q002" to "Avoid erroneous Q002 error message for single-quote docstrings" by @charliermarsh on 2023-01-12 01:01_

---

_Merged by @charliermarsh on 2023-01-12 01:01_

---

_Closed by @charliermarsh on 2023-01-12 01:01_

---

_Comment by @charliermarsh on 2023-01-12 01:12_

@colin99d - If you're interested in a follow-up...

If we _are_ using a triple quote, I don't _think_ we should allow you to invert the quote style. That is, if you're preferring `"`, and you do this:

```py
def f():
  '''This has a quote " in it.'''
```

I think, perhaps, that should be considered a violation, since you can just do this:

```py
def f():
  """This has a quote " in it."""
```

So the flexibility we added here, IMO, should only apply when using single-quoted docstrings.

---

_Comment by @colin99d on 2023-01-12 01:16_

> @colin99d - If you're interested in a follow-up...
> 
> If we _are_ using a triple quote, I don't _think_ we should allow you to invert the quote style. That is, if you're preferring `"`, and you do this:
> 
> ```python
> def f():
>   '''This has a quote " in it.'''
> ```
> 
> I think, perhaps, that should be considered a violation, since you can just do this:
> 
> ```python
> def f():
>   """This has a quote " in it."""
> ```
> 
> So the flexibility we added here, IMO, should only apply when using single-quoted docstrings.

Makes sense to me! Thanks for the explanation.

---

_Branch deleted on 2023-01-12 01:16_

---

_Comment by @charliermarsh on 2023-01-12 01:18_

Another way of looking at it is... what would Black convert?

Black changes this to double quotes:

```py
def f():
    '''Some "docstring.'''
```

But not this:

```py
x = 'here is a quote " inside'
```

---

_Comment by @colin99d on 2023-01-12 01:20_

> Another way of looking at it is... what would Black convert?
> 
> Black changes this to double quotes:
> 
> ```python
> def f():
>     '''Some "docstring.'''
> ```
> 
> But not this:
> 
> ```python
> x = 'here is a quote " inside'
> ```

From now on I will just let "what would black do" answer formatting ambiguity questions I have.

---

_Comment by @charliermarsh on 2023-01-12 01:21_

In our case, though, we also the opposite, if the user has `Quote::Single` set :)

---
