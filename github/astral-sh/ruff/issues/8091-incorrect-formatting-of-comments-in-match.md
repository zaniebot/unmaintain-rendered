---
number: 8091
title: "Incorrect formatting of comments in `match` statement for tuple without parentheses"
type: issue
state: closed
author: doraut
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-10-20T09:38:48Z
updated_at: 2023-10-22T23:58:35Z
url: https://github.com/astral-sh/ruff/issues/8091
synced_at: 2026-01-07T13:12:15-06:00
---

# Incorrect formatting of comments in `match` statement for tuple without parentheses

---

_Issue opened by @doraut on 2023-10-20 09:38_

**Version**: 0.1.1
**Command**: `ruff format --isolated example.py`
**Playground**:  [Example](https://play.ruff.rs/666bea7e-0cda-445c-8153-6eb6839b0e57)

## Example
```python
def fizzbuzz(n):
    match n % 3, n % 5:
        case 0, 0:
            # n is divisible by both 3 and 5
            print("FizzBuzz")
        case 0, _:
            # n is divisible by 3, but not 5
            print("Fizz")
        case _, 0:
            # n is divisible by 5, but not 3
            print("Buzz")
        case _:
            print(n)
```

This is formatted to
```python
def fizzbuzz(n):
    match (
        n % 3,
        n % 5,
        # n is divisible by both 3 and 5
        # n is divisible by 3, but not 5
        # n is divisible by 5, but not 3
    ):
        case 0, 0:
            print("FizzBuzz")
        case 0, _:
            print("Fizz")
        case _, 0:
            print("Buzz")
        case _:
            print(n)
```
Adding parentheses around the matched tuple (`match n % 3, n % 5` -> `match (n % 3, n % 5)`) results in expected formatting.


---

_Comment by @MichaReiser on 2023-10-20 11:43_

Uhm that's weird. Thanks for reporting 

---

_Label `bug` added by @MichaReiser on 2023-10-20 11:43_

---

_Label `formatter` added by @MichaReiser on 2023-10-20 11:43_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-10-20 11:43_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-21 02:40_

---

_Comment by @charliermarsh on 2023-10-21 02:46_

This is a parser bug -- the ranges are off.

---

_Referenced in [astral-sh/ruff#8101](../../astral-sh/ruff/pulls/8101.md) on 2023-10-21 02:55_

---

_Closed by @charliermarsh on 2023-10-22 23:58_

---
