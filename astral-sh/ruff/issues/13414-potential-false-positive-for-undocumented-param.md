```yaml
number: 13414
title: "Potential false positive for `undocumented-param` (`D417)`"
type: issue
state: open
author: harupy
labels:
  - bug
  - docstring
assignees: []
created_at: 2024-09-20T02:46:20Z
updated_at: 2024-10-04T05:13:29Z
url: https://github.com/astral-sh/ruff/issues/13414
synced_at: 2026-01-12T15:54:53Z
```

# Potential false positive for `undocumented-param` (`D417)`

---

_@harupy_

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


I have the following function. The docstring has a nested `Example` section in the `x` description.

```python
def f(x, y):
    """
    Args:
        x: This is x.

            Example:

            .. code-block:: python

                import mlflow

                mlflow.log_param("x", x)

        y: This is y.
    """
```

This function violates `D417`:

```
Missing argument description in the docstring for `f`: `y`
```

Reproduction:

https://play.ruff.rs/714c1337-edee-40c2-857a-435dd9a5d243

---

_Label `bug` added by @charliermarsh on 2024-09-20 02:52_

---

_Label `docstring` added by @charliermarsh on 2024-09-20 02:52_

---

_Renamed from "Potential false positive for `undocumented-param (D417)`" to "Potential false positive for `undocumented-param` (`D417)`" by @dhruvmanila on 2024-10-04 05:13_

---
