---
number: 9292
title: "Ruff doesn't detect unsorted imports if there is an extra newline between imports and next block"
type: issue
state: closed
author: discentem
labels: []
assignees: []
created_at: 2023-12-27T05:04:48Z
updated_at: 2023-12-27T09:08:36Z
url: https://github.com/astral-sh/ruff/issues/9292
synced_at: 2026-01-07T13:12:15-06:00
---

# Ruff doesn't detect unsorted imports if there is an extra newline between imports and next block

---

_Issue opened by @discentem on 2023-12-27 05:04_

> A minimal code snippet that reproduces the bug.

```shell
% ruff check .      
% cat timestamp/timestamp.py 
```

```python
import datetime

# <--- extra line
def is_expired(unix_timestamp: int) -> bool:
    timestamp = datetime.datetime.fromtimestamp(unix_timestamp)
    current_time = datetime.datetime.now()
    return current_time > timestamp
```
```shell
% cat timestamp/timestamp.py
```

```python
import datetime
# <--- extra line now removed
def is_expired(unix_timestamp: int) -> bool:
    timestamp = datetime.datetime.fromtimestamp(unix_timestamp)
    current_time = datetime.datetime.now()
    return current_time > timestamp
```
```shell
% ruff check .              
timestamp/timestamp.py:1:1: I001 [*] Import block is un-sorted or un-formatted
Found 1 error.
[*] 1 fixable with the `--fix` option.
```

> The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
`ruff check .` and `ruff check . --fix`
> The current Ruff settings (any relevant sections from your `pyproject.toml`).

```toml
[tool.ruff.lint]
select = [
    "ANN001", # https://docs.astral.sh/ruff/rules/missing-type-function-argument/
    "F401",   # https://docs.astral.sh/ruff/rules/unused-import/
    "I001",   # https://docs.astral.sh/ruff/rules/unsorted-imports/
    "I002",   # https://docs.astral.sh/ruff/rules/missing-required-import/
    "F821",   # https://docs.astral.sh/ruff/rules/undefined-name/
    "F823",   # https://docs.astral.sh/ruff/rules/local-variable-undefined/
    "F841",   # https://docs.astral.sh/ruff/rules/local-variable-unused/
    "F842",   # https://docs.astral.sh/ruff/rules/unused-annotation/
]
```

> The current Ruff version (`ruff --version`).
`ruff 0.1.5`


---

_Renamed from "Ruff doesn't detect unsorted imports if there is an extra space between imports and next block" to "Ruff doesn't detect unsorted imports if there is an extra newline between imports and next block" by @discentem on 2023-12-27 05:04_

---

_Closed by @discentem on 2023-12-27 09:08_

---
