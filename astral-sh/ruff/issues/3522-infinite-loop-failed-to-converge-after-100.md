```yaml
number: 3522
title: "[Infinite Loop] Failed to converge after 100 iterations."
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-03-14T20:24:56Z
updated_at: 2023-03-19T18:29:15Z
url: https://github.com/astral-sh/ruff/issues/3522
synced_at: 2026-01-12T15:54:43Z
```

# [Infinite Loop] Failed to converge after 100 iterations.

---

_@qarmin_

Ruff 0.0.255
Command
```
ruff ~/Desktop/Ruff/Broken/AcquireFile6.py --config /home/rafal/Desktop/ruff.toml --fix --no-cache
```
Settings
```
select = ["ALL"]
```

[AcquireFile6.py.zip](https://github.com/charliermarsh/ruff/files/10973446/AcquireFile6.py.zip)

produce error 
```
error: Failed to converge after 100 iterations.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `AcquireFile6.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```

---

_Label `bug` added by @charliermarsh on 2023-03-14 21:00_

---

_Comment by @vlindhol on 2023-03-19 15:45_

I took a look at this, and managed to narrow it down to the following minimal test case:

```python
class A:
    """
\ a
    """
```

With the config:

```toml
select = [
  "D207", # bad docstring indentation
  "W605", # invalid escape sequence
]
```

Those two rules clash in the following way

1. `W605` schedules an insertion of a `\` at column 0
2. `D207` schedules an insertion of `....` (four spaces) at column 0
3. End result: `\ a` -> `\` + `    ` + `\ a` -> `\    \ a`
4. GOTO 1

The culprit is the fairly simplistic way that "fix overlaps" are calculated. An easy workaround is for `W605` to insert the `\` in column 1 instead. I'll make a PR about it!

---

_Comment by @charliermarsh on 2023-03-19 15:52_

Thank you :heart:

---

_Assigned to @vlindhol by @charliermarsh on 2023-03-19 15:52_

---

_Closed by @charliermarsh on 2023-03-19 18:29_

---
