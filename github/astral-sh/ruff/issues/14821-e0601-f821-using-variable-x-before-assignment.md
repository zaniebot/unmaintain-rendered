---
number: 14821
title: "E0601/F821: Using variable 'x' before assignment (used-before-assignment) not working"
type: issue
state: closed
author: FrancoisMasson1990
labels: []
assignees: []
created_at: 2024-12-06T15:56:26Z
updated_at: 2024-12-06T16:26:48Z
url: https://github.com/astral-sh/ruff/issues/14821
synced_at: 2026-01-07T13:12:16-06:00
---

# E0601/F821: Using variable 'x' before assignment (used-before-assignment) not working

---

_Issue opened by @FrancoisMasson1990 on 2024-12-06 15:56_

ruff does not seem to be able to detect unbound variables in python.

Example:

```python
def test() -> int:
    try:
        continue
        x = 1
    except Exception:
        pass
    finally:
        return x
````

Return no error of unbound variables

---

_Comment by @MichaReiser on 2024-12-06 16:26_

I think this is the same as https://github.com/astral-sh/ruff/issues/2493 and https://github.com/astral-sh/ruff/issues/6902 so I'll close this in favor of an existing issue. 

The problem is that Ruff's analysis isn't branch-sensitive. It doesn't know that you can reach the `finally` with `x` unbound if the `except` branch is taken at runtime (and only if the except branch is taken before `x` gets declared). 

We work on an improved analysis engine that can catch this kind of error. 

---

_Closed by @MichaReiser on 2024-12-06 16:26_

---
