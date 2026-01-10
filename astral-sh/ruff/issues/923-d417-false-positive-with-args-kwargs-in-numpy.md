```yaml
number: 923
title: "D417: false positive with *args/**kwargs in numpy docstrings"
type: issue
state: closed
author: tlambert03
labels:
  - bug
assignees: []
created_at: 2022-11-27T16:29:37Z
updated_at: 2022-11-28T11:26:52Z
url: https://github.com/astral-sh/ruff/issues/923
synced_at: 2026-01-10T12:09:58Z
```

# D417: false positive with *args/**kwargs in numpy docstrings

---

_Issue opened by @tlambert03 on 2022-11-27 16:29_

In the [specification for numpy docstrings](https://numpydoc.readthedocs.io/en/latest/format.html#parameters), they state:

> When documenting variable length positional, or keyword arguments, leave the leading star(s) in front of the name:
> 
> ```
> *args : tuple
>     Additional arguments should be passed as keyword arguments
> **kwargs : dict, optional
>     Extra arguments to `metric`: refer to each metric documentation for a
>     list of all possible arguments.
> ```

but if you do that, ruff is unhappy:

```python
def f(*stuff: int):
    """Summary.

    Parameters
    ----------
    *stuff : int
        all the stuff.
    """
```
```
Missing argument description in the docstring: `stuff` Ruff(D417)
```

(if you remove the star in the docstrings, the error goes away)

---

_Comment by @charliermarsh on 2022-11-27 16:45_

Oh interesting, thanks! Will fix.

---

_Label `bug` added by @charliermarsh on 2022-11-27 20:38_

---

_Closed by @charliermarsh on 2022-11-28 03:08_

---

_Comment by @charliermarsh on 2022-11-28 05:40_

This is going out in [v0.0.142](https://github.com/charliermarsh/ruff/releases/tag/v0.0.142).

---

_Comment by @tlambert03 on 2022-11-28 11:26_

So fast :). Thanks again!

---
