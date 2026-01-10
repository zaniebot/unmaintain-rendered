```yaml
number: 9397
title: "Ruff does not report the `assignment-from-no-return` error"
type: issue
state: closed
author: behnazh-w
labels: []
assignees: []
created_at: 2024-01-05T07:02:08Z
updated_at: 2024-01-05T10:49:44Z
url: https://github.com/astral-sh/ruff/issues/9397
synced_at: 2026-01-10T11:09:51Z
```

# Ruff does not report the `assignment-from-no-return` error

---

_Issue opened by @behnazh-w on 2024-01-05 07:02_

I was expecting ruff to report an error similar to Pylint's [assignment-from-no-return (E1111)](https://pylint.pycqa.org/en/v3.0.3/user_guide/checkers/features.html#typecheck-checker-messages) for the following code, however, I get no errors.
```python
def print_sorted_list() -> None:
    """Print a sorted list."""
    sorted_list = [1, 2, 3].sort() # `list.sort()` is an in-place function and doesn't return anything.
    print(sorted_list)
```

The command I invoked:
```bash
ruff /path/to/file.py
```
The current Ruff settings (any relevant sections from your `pyproject.toml`).
```toml
[tool.ruff]
select = [
    "C4",
    "E",
    "F",
    "W",
    "B",
    "I",
    "G",
    "RUF",
    "UP",
    "ISC",
    "PERF",
    "PL",
]
```
The current Ruff version (`ruff --version`):
```
ruff 0.1.11
```

The error I get by running Pylint v3.0.3:
```
E1111: Assigning result of a function call, where the function has no return (assignment-from-no-return)
```


---

_Comment by @tjkuson on 2024-01-05 10:11_

I don't think that Pylint rule hasn't been implemented yet. You can view the progress of which Pylint rules have been implemented here: #970.

---

_Comment by @behnazh-w on 2024-01-05 10:49_

Thanks for the reference. Closing this issue as a duplicate of https://github.com/astral-sh/ruff/issues/970.

---

_Closed by @behnazh-w on 2024-01-05 10:49_

---
