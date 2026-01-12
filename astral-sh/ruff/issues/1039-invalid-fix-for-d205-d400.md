```yaml
number: 1039
title: Invalid fix for D205+D400
type: issue
state: closed
author: serjflint
labels:
  - bug
assignees: []
created_at: 2022-12-04T16:50:48Z
updated_at: 2022-12-04T18:56:52Z
url: https://github.com/astral-sh/ruff/issues/1039
synced_at: 2026-01-12T15:54:40Z
```

# Invalid fix for D205+D400

---

_@serjflint_

pyproject.toml

```toml
[tool.ruff]
extend-select = ["D"]
extend-ignore = [
    "D203",
    "D212",
    "D213",
    "D214",
    "D215",
    "D404",
    "D405",
    "D406",
    "D407",
    "D408",
    "D409",
    "D410",
    "D411",
    "D413",
    "D415",
    "D416",
    "D417",
]

```

test_ruff.py

```python
"""Stub."""


class Ruffed:
    """Stub."""

    def __call__(self, a, b) -> None:
        """
        :param a: table rows
        :param b: table rows
        """
        pass

```

Run ruff 0.0.156 with fix:

```bash
ruff test_ruff.py --fix
```

It adds one hundred empty lines after line 8

---

_Comment by @charliermarsh on 2022-12-04 17:01_

One hundred empty lines! Not good! Will take a look, thanks for filing.

---

_Label `bug` added by @charliermarsh on 2022-12-04 17:01_

---

_Comment by @charliermarsh on 2022-12-04 17:29_

(I’m guessing that the fixes are somehow conflicting and so it’s applying them repeatedly, up to 100 iterations.)

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-04 18:36_

---

_Comment by @charliermarsh on 2022-12-04 18:56_

This is resolved by https://github.com/charliermarsh/ruff/pull/1041, but the behavior probably won't be quite what you want. The "right" thing to do, AFAICT, is detect that `:param a: table rows` is the "summary line", and add a newline after that line. That is at least stable and consistent with the rule logic.

---

_Closed by @charliermarsh on 2022-12-04 18:56_

---
