```yaml
number: 792
title: False positive for D417
type: issue
state: closed
author: PhilReinhold
labels:
  - bug
assignees: []
created_at: 2022-11-17T17:25:57Z
updated_at: 2022-12-12T20:12:29Z
url: https://github.com/astral-sh/ruff/issues/792
synced_at: 2026-01-10T12:06:14Z
```

# False positive for D417

---

_Issue opened by @PhilReinhold on 2022-11-17 17:25_

Running ruff v0.0.124, python 3.10.6, on OSX 12.6.1

Linting the following code:

```python
def f(x):
    """Do f.

    Args:
        x: the value

    Returns: the value
    """
    return x
```

Reports 
```
test.py:1:1: D417 Missing argument description in the docstring: `x`
```

The error goes away if the `Returns` block is reformatted with a new line.

```python
def f(x):
    """Do f.

    Args:
        x: the value

    Returns:
        the value
    """
    return x
```



---

_Comment by @charliermarsh on 2022-11-17 17:27_

Great, thank you for the report!

---

_Label `bug` added by @charliermarsh on 2022-11-17 17:27_

---

_Comment by @charliermarsh on 2022-11-17 17:29_

(For some reason we're not detecting NumPy-style docstrings in the first example, which seems to be the root cause.)

---

_Comment by @charliermarsh on 2022-11-17 17:41_

(Err, wait, Flake8 doesn't detect NumPy-style docstrings either, so probably a bug in the Google-style parser.)

---

_Closed by @charliermarsh on 2022-11-17 17:55_

---

_Comment by @PhilReinhold on 2022-12-12 20:12_

Thanks for the fix! However, I've noticed that the error returns if there is more than one argument present:

```python
# test.py
def f(x, y, z):
    """Do f.

    Args:
        x: the value
        y: Another thing
        z: A final argument

    Returns: the value
    """
    return x
```

```
$ ruff --version
ruff 0.0.177

$ ruff --select D test.py
Found 1 error(s).
test.py:1:1: D417 Missing argument descriptions in the docstring: `y`, `z`
```

The error again goes away if the `Returns` block gets a newline. Insisting on a newline there is actually fine for me, but the confusing error message is still unfortunate.


---
