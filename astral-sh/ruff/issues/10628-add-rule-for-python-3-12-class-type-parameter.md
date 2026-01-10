```yaml
number: 10628
title: Add rule for Python 3.12+ class type parameter lists
type: issue
state: closed
author: autinerd
labels: []
assignees: []
created_at: 2024-03-27T15:03:08Z
updated_at: 2024-03-28T12:24:54Z
url: https://github.com/astral-sh/ruff/issues/10628
synced_at: 2026-01-10T11:09:52Z
```

# Add rule for Python 3.12+ class type parameter lists

---

_Issue opened by @autinerd on 2024-03-27 15:03_

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

Hello!

Since Python 3.12, classes in this syntax:

```python
class A(Generic[T]):
    ...
```

can be written as this:

```python
class A[T]:
    ...
```

It would be cool to have an upgrade rule for this.

---

_Comment by @AlexWaygood on 2024-03-28 12:24_

Thanks for the suggestion! Closing as a duplicate of #4617

---

_Closed by @AlexWaygood on 2024-03-28 12:24_

---
