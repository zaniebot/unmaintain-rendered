```yaml
number: 11603
title: C400 unsafe fix is more unsafe than the documentation suggests
type: issue
state: closed
author: simonpercivall
labels:
  - bug
  - fixes
assignees: []
created_at: 2024-05-29T20:43:21Z
updated_at: 2024-05-30T03:26:57Z
url: https://github.com/astral-sh/ruff/issues/11603
synced_at: 2026-01-12T15:54:51Z
```

# C400 unsafe fix is more unsafe than the documentation suggests

---

_@simonpercivall_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Ruff 0.45

The C400 unsafe fix says that that it sometimes drops comments. It may also generate broken code:

`ruff check fix --unsafe-fixes --select C400`
```python
list(
    (1 for _ in range(1))
)
```

expected
```python
[
    1 for _ in range(1)
]  # [1]
```

actual
```python
[
    (1 for _ in range(1))
]  # [<generator object <genexpr> at ...>
```

---

_Label `bug` added by @AlexWaygood on 2024-05-29 22:06_

---

_Label `fixes` added by @AlexWaygood on 2024-05-29 22:06_

---

_Comment by @charliermarsh on 2024-05-30 00:04_

👍 Yeah that's a bug (rather than a question of fix safety). Thanks.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-30 02:35_

---

_Closed by @charliermarsh on 2024-05-30 03:26_

---
