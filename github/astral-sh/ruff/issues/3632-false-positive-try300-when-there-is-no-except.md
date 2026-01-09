---
number: 3632
title: "False positive `TRY300` when there is no `except` clause"
type: issue
state: closed
author: Czaki
labels:
  - bug
assignees: []
created_at: 2023-03-20T17:23:24Z
updated_at: 2023-03-20T20:55:30Z
url: https://github.com/astral-sh/ruff/issues/3632
synced_at: 2026-01-07T13:12:14-06:00
---

# False positive `TRY300` when there is no `except` clause

---

_Issue opened by @Czaki on 2023-03-20 17:23_

In napari we have such a function https://github.com/napari/napari/blob/ab885957fb5a2220a3078851ad7dc97a7a82cc05/napari/utils/misc.py#L253

```python
def formatdoc(obj):
    """Substitute globals and locals into an object's docstring."""
    frame = inspect.currentframe().f_back
    try:
        obj.__doc__ = obj.__doc__.format(
            **{**frame.f_globals, **frame.f_locals}
        )
        return obj
    finally:
        del frame
```

The `TRY300` is reported on `return obj`. But the else clause is available only when the `except` clause is present (there needs to be at least one). So `TRY300` should not be reported if there is no except clause. Or it should have different text describing that. 

Currently:

```
TRY300 Consider moving this statement to an `else` block
```

Possible:

```
TRY300 Consider moving this statement after `try: finally:`
```


---

_Referenced in [napari/napari#5590](../../napari/napari/pulls/5590.md) on 2023-03-20 17:25_

---

_Comment by @JonathanPlasse on 2023-03-20 18:37_

I would like to work on it.

---

_Label `bug` added by @charliermarsh on 2023-03-20 19:02_

---

_Referenced in [astral-sh/ruff#3634](../../astral-sh/ruff/pulls/3634.md) on 2023-03-20 20:20_

---

_Closed by @charliermarsh on 2023-03-20 20:55_

---
