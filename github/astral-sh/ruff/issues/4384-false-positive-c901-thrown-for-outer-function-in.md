---
number: 4384
title: "False positive `C901` thrown for outer function in closure"
type: issue
state: open
author: jamesbraza
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-05-11T21:45:05Z
updated_at: 2025-07-10T06:07:56Z
url: https://github.com/astral-sh/ruff/issues/4384
synced_at: 2026-01-07T13:12:14-06:00
---

# False positive `C901` thrown for outer function in closure

---

_Issue opened by @jamesbraza on 2023-05-11 21:45_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Please see this snippet:

```python
from functools import wraps


def wrapper(f):
    @wraps(f)
    def wrapped_f(*args, **kwargs):
        if 1 == 1:
            pass
        if 1 == 1:
            pass
        if 1 == 1:
            pass
        return f(*args, **kwargs)

    return wrapped_f
```

And the below `pyproject.toml`

```toml
[tool.ruff.mccabe]
max-complexity = 3

[tool.flake8]
max-complexity = 3
```

With `ruff==0.0.265` will print:

```bash
> ruff --select=C901 a.py
a.py:4:5: C901 `wrapper` is too complex (5 > 3)
a.py:6:9: C901 `wrapped_f` is too complex (4 > 3)
Found 2 errors.
> flake8 a.py
a.py:4:1: C901 'wrapper' is too complex (5)
```

The 2nd error about `wrapped_f` if correct, as its cyclomatic complexity is above 3.  However, I think the 1st error about the `wrapper` (outer) function is a false positive, as it has a cyclomatic complexity ≤ 3.

In other words, I think `ruff` has a false positive `C901` for closures, only the inner should throw.

For what it's worth, `flake8==6.0.0` does not have this false positive, but I think they mark the extra complexity in the wrong place (at `wrapper`, not `wrapped_f`).

---

_Label `question` added by @charliermarsh on 2023-05-12 01:33_

---

_Comment by @charliermarsh on 2023-05-12 01:34_

I think I agree with you, though I'd love to get other opinions since it would be a deviation from Flake8 (or rather, McCabe).

---

_Label `rule` added by @charliermarsh on 2023-07-10 01:14_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:14_

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:14_

---

_Referenced in [astral-sh/ruff#19228](../../astral-sh/ruff/issues/19228.md) on 2025-07-09 10:46_

---

_Comment by @travis-leith on 2025-07-10 06:07_

> I think I agree with you, though I'd love to get other opinions since it would be a deviation from Flake8 (or rather, McCabe).

@charliermarsh have a look at https://github.com/astral-sh/ruff/issues/19228. I think it is hard to argue that this function is complex. It should not be flagged imo.

---
