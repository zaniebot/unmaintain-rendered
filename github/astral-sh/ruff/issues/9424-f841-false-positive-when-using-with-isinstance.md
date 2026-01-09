---
number: 9424
title: F841 - False positive when using with isinstance()
type: issue
state: closed
author: MamadouSDiallo
labels:
  - question
assignees: []
created_at: 2024-01-07T17:30:48Z
updated_at: 2024-01-07T21:49:15Z
url: https://github.com/astral-sh/ruff/issues/9424
synced_at: 2026-01-07T13:12:15-06:00
---

# F841 - False positive when using with isinstance()

---

_Issue opened by @MamadouSDiallo on 2024-01-07 17:30_

In Python, it is common to use `isinstance()` to check for type or class. If we do not do anything with the class or type name then it is flagged as F841 (unused variable) which is a false positive.  

```python
def fit(y: DirectEst | Array, x: IndepVars, method: FitMethod):
    match y:
        case isinstance(y, DirectEst):
            return _fit(y=y.est, x=x, method=method)
        case isinstance(y, Array):
            return _fit(y=y, x=x, method=method)
        case _:
            raise TypeError("The type of `y` is not supported!")
```


---

_Comment by @zanieb on 2024-01-07 18:25_

Thanks for the report! This looks like an issue with `case` statements specifically.

```
❯ ruff check example.py --select F841
example.py:3:28: F841 Local variable `DirectEst` is assigned to but never used
example.py:5:28: F841 Local variable `Array` is assigned to but never used
```

but there are no violations in

```python
def fit(y: DirectEst | Array, x: IndepVars, method: FitMethod):
    if isinstance(y, DirectEst):
        return _fit(y=y.est, x=x, method=method)
    elif isinstance(y, Array):
        return _fit(y=y, x=x, method=method)
    else:
        raise TypeError("The type of `y` is not supported!")
```

---

_Comment by @charliermarsh on 2024-01-07 18:56_

I think Ruff is correct here? That code doesn't behave the way it might appear. You can't put arbitrary Python expressions in a `case` statement. Python interprets `case isinstance(y, DirectEst)` as "match against a class named `instance`", not "match against the result of checking `isinstance(y, DirectEst)`.

For example, given:

```python
class DirectEst:
    pass

class Array:
    pass

def fit(y: DirectEst | Array):
    match y:
        case isinstance(y, DirectEst):
            print("DirectEst")
        case isinstance(y, Array):
            print("Array")
        case _:
            raise TypeError("The type of `y` is not supported!")

fit(DirectEst())
```

I get:

```
Traceback (most recent call last):
  File "/Users/crmarsh/workspace/ruff/foo.py", line 16, in <module>
    fit(DirectEst())
  File "/Users/crmarsh/workspace/ruff/foo.py", line 9, in fit
    case isinstance(y, DirectEst):
         ^^^^^^^^^^^^^^^^^^^^^^^^
TypeError: called match pattern must be a class
```


---

_Comment by @charliermarsh on 2024-01-07 18:58_

You might be thinking of something like:

```python
def fit(y: DirectEst | Array, x: IndepVars, method: FitMethod):
    match y:
        case DirectEst():
            return _fit(y=y.est, x=x, method=method)
        case Array():
            return _fit(y=y, x=x, method=method)
        case _:
            raise TypeError("The type of `y` is not supported!")
```

Which Ruff correctly does _not_ flag for `F841`.

---

_Label `question` added by @charliermarsh on 2024-01-07 18:58_

---

_Comment by @MamadouSDiallo on 2024-01-07 21:48_

Indeed, if I replace the match by and if/else then it works fine and Ruff does not complain. The issue is the weird Python type system not Ruff. 
Best.

---

_Closed by @MamadouSDiallo on 2024-01-07 21:49_

---
