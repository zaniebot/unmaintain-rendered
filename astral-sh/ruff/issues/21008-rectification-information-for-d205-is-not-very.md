```yaml
number: 21008
title: Rectification information for D205 is not very helpful
type: issue
state: open
author: varchasgopalaswamy
labels:
  - diagnostics
assignees: []
created_at: 2025-10-21T01:02:49Z
updated_at: 2025-10-21T15:12:38Z
url: https://github.com/astral-sh/ruff/issues/21008
synced_at: 2026-01-10T11:10:00Z
```

# Rectification information for D205 is not very helpful

---

_Issue opened by @varchasgopalaswamy on 2025-10-21 01:02_

### Summary

The D205 rule, which is certainly a useful one to have, doesn't have an especially helpful message from Ruff.

Minimum example: 

```python

def foo() -> None:
    """
    A placeholder docstring
    that will trigger D205
    """
```

The error that ruff will generate from this is 
```
D205 1 blank line required between summary line and description
  --> utils.py:18:5
   |
17 |   def foo() -> None:
18 | /     """
19 | |     A placeholder docstring
20 | |     that will trigger D205
21 | |     """
   | |_______^
   |
help: Insert single blank line

```

However, this isn't really a helpful message, and doesn't really let the user know what needed to be changed. Should the blank line be inserted like this? 

```python

def foo() -> None:
    """

    A placeholder docstring
    that will trigger D205
    """
```
or like this?

```python

def foo() -> None:
    """
    A placeholder docstring
    that will trigger D205

    """
```

or like this? (This fixes D205, but the docstring is now totally mangled!)

```python

def foo() -> None:
    """
    A placeholder docstring

    that will trigger D205
    """
```

The correct fix would have been 

```python

def foo() -> None:
    """A placeholder docstring that will trigger D205
    """
```

but the error message doesn't provide the necessary hints. The ruff documentation for D205, however, does have the necessary information: 

"[PEP 257](https://peps.python.org/pep-0257/) recommends that multi-line docstrings consist of "a summary line just like a one-line docstring, followed by a blank line, followed by a more elaborate description."

Suggestion: Have the error description just repeat the above verbatim, since it clearly indicates what needs to be done. 


---

_Comment by @ntBre on 2025-10-21 13:23_

I think the whole excerpt from the docs might be a bit long, but making the `help` message something like `Insert a single blank line after the first line of the docstring` might help.

---

_Label `diagnostics` added by @ntBre on 2025-10-21 13:23_

---

_Comment by @varchasgopalaswamy on 2025-10-21 15:12_

The problem is that inserting a single blank line isn't the fix! In this case, the problem is that the docstring should have become a single line docstring, since it's just the one-line summary split across multiple lines. My guess is that some code looks like this to try to respect terminal line limits. How about `Multi-line docstrings should be a one-line summary, a blank line, and a detailed description.`

---
