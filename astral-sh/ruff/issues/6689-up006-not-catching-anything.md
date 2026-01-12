```yaml
number: 6689
title: UP006 not catching anything
type: issue
state: closed
author: Garrett-R
labels: []
assignees: []
created_at: 2023-08-19T05:37:58Z
updated_at: 2023-08-19T05:53:15Z
url: https://github.com/astral-sh/ruff/issues/6689
synced_at: 2026-01-12T15:54:46Z
```

# UP006 not catching anything

---

_@Garrett-R_

## Steps to repro

- create an empty directory
- create a Python 3.11 virtualenv in that directory and activate it (I have Python 3.11.4)
- Do `pip install ruff`  (got version `0.0.285`)
- Create this file called `repro.py`:

```python
from typing import List

def my_func(x: List[str]) -> None:
    pass
```

- Run the command: `ruff check --select ARG001,UP006 repro.py`

## Expected Behavior

There should be a [UP006 error](https://beta.ruff.rs/docs/rules/non-pep585-annotation/) among the output.

## Actual Behavior

The output is:
```python
$ ruff check --select ARG001,UP006 repro.py
repro.py:3:13: ARG001 Unused function argument: `x`
Found 1 error.
```

## System

- Ubuntu 22.04.1



---

_Comment by @Garrett-R on 2023-08-19 05:53_

Oh, I see, I need to set [`target-version`](https://beta.ruff.rs/docs/settings/#target-version) to my current Python version.

---

_Closed by @Garrett-R on 2023-08-19 05:53_

---
