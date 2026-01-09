---
number: 4440
title: min/max flattening broken for iterables
type: issue
state: closed
author: nijel
labels:
  - bug
assignees: []
created_at: 2023-05-15T14:32:47Z
updated_at: 2023-05-15T15:31:12Z
url: https://github.com/astral-sh/ruff/issues/4440
synced_at: 2026-01-07T13:12:14-06:00
---

# min/max flattening broken for iterables

---

_Issue opened by @nijel on 2023-05-15 14:32_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

min/max flattening (when using `ruff --fix`) is broken when arguments are iterables.

Minimal reproducer:

```python
print(max(max([1, 2, 3]), max([3, 4, 5])))
```

Is turned into:

```python
print(max([1, 2, 3], [3, 4, 5]))
```

The original code yields `5`, while "fixed" yields `[3, 4, 5]`.

Introduced by https://github.com/charliermarsh/ruff/pull/4200

---

_Referenced in [astral-sh/ruff#4200](../../astral-sh/ruff/pulls/4200.md) on 2023-05-15 14:35_

---

_Comment by @charliermarsh on 2023-05-15 15:31_

Fixed by #4412.

---

_Closed by @charliermarsh on 2023-05-15 15:31_

---

_Label `bug` added by @charliermarsh on 2023-05-15 15:31_

---
