```yaml
number: 8584
title: "`ruff check --diff` passes when `ruff check` finds an error"
type: issue
state: closed
author: stinodego
labels:
  - question
assignees: []
created_at: 2023-11-09T14:00:03Z
updated_at: 2023-11-10T11:57:04Z
url: https://github.com/astral-sh/ruff/issues/8584
synced_at: 2026-01-10T11:09:50Z
```

# `ruff check --diff` passes when `ruff check` finds an error

---

_Issue opened by @stinodego on 2023-11-09 14:00_

Example file:

```python
# repro.py
def hello(x: int):
    """
    Sup.

    Parameters
    ----------
    y
        Hi

    """
```
Running `ruff check repro.py` gives the following output:
```
repro.py:1:5: D417 Missing argument description in the docstring for `hello`: `x`
Found 1 error.
```

Running `ruff check --diff repro.py` **gives no output and passes**.

#### Settings

```toml
# pyproject.toml
[tool.ruff]
select = [
  "D", # flake8-docstrings
]
ignore = [
  "D1",
  "D2",
]
```

Ruff version: `0.1.5`

---

_Comment by @zanieb on 2023-11-09 14:45_

Hi! This is the expected behavior:

```
      --diff
          Avoid writing any fixed files back; instead, output a diff for each changed file to stdout. Implies `--fix-only`
      --fix-only
          Apply fixes to resolve lint violations, but don't report on leftover violations. 
```

I think what you're looking for will be best resolved by #7352 

---

_Label `question` added by @zanieb on 2023-11-09 14:45_

---

_Comment by @stinodego on 2023-11-09 15:01_

Right, it seems I misunderstood what `--diff` does. Now that I think about it more, it makes sense.

What would then be the recommended way to run ruff in a CI pipeline? Maybe the following?

```
ruff check --no-fix .
ruff format --diff .
```

---

_Closed by @stinodego on 2023-11-09 15:01_

---
