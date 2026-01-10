```yaml
number: 1291
title: False positive for D417 (Part 2)
type: issue
state: closed
author: PhilReinhold
labels:
  - bug
assignees: []
created_at: 2022-12-19T18:48:04Z
updated_at: 2022-12-20T18:26:58Z
url: https://github.com/astral-sh/ruff/issues/1291
synced_at: 2026-01-10T12:05:25Z
```

# False positive for D417 (Part 2)

---

_Issue opened by @PhilReinhold on 2022-12-19 18:48_

Opening a new issue here to track the follow-on comment I left here:

https://github.com/charliermarsh/ruff/issues/792#issuecomment-1347239427

Transcribing that comment:

Thanks for the fix for #792! However, I've noticed that the error returns if there is more than one argument present:
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

$ ruff --version
ruff 0.0.177

$ ruff --select D test.py
Found 1 error(s).
test.py:1:1: D417 Missing argument descriptions in the docstring: `y`, `z`

The error again goes away if the Returns block gets a newline. Insisting on a newline there is actually fine for me, but the confusing error message is still unfortunate.

---

_Comment by @charliermarsh on 2022-12-19 18:53_

Thank you, sorry to miss that one.

---

_Label `bug` added by @charliermarsh on 2022-12-19 18:53_

---

_Closed by @charliermarsh on 2022-12-19 21:39_

---

_Comment by @charliermarsh on 2022-12-20 18:26_

Going out in [v0.0.189](https://github.com/charliermarsh/ruff/releases/tag/v0.0.189).

---
