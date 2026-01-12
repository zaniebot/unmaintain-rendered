```yaml
number: 11616
title: PLR1701 fix is not safe (especially together with SIM114)
type: issue
state: closed
author: simonpercivall
labels: []
assignees: []
created_at: 2024-05-30T09:56:50Z
updated_at: 2024-05-30T18:05:25Z
url: https://github.com/astral-sh/ruff/issues/11616
synced_at: 2026-01-12T15:54:51Z
```

# PLR1701 fix is not safe (especially together with SIM114)

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

`PLR1701` is mentioned in passing in https://github.com/astral-sh/ruff/issues/10245.

`PLR1701` will merge repeated `isinstance` calls and also rewrite literal tuples into unions for modern Python versions. But an `isinstance` check with a variable bound `tuple` will _also_ be merged into the union, and that will cause a `TypeError` at runtime.

This especially interacts badly with `SIM114` where you _could_ have defined two `if` arms, one with a variable bound `tuple`, and one with a literal `tuple`, and it'll get merged and wrongly rewritten as a union. Example below.

Ruff 0.45

`pyproject.toml`:
```toml
[tool.ruff]
target-version = "py311"
```

`ruff check --select SIM114,PLR1701 --fix`
```python
TYPES = (dict, list)

if isinstance(None, TYPES):
    pass
elif isinstance(None, set | float):
    pass
```

actual
```python
TYPES = (dict, list)

if isinstance(None, TYPES | set | float):
    pass

# Traceback (most recent call last):
#   File "PLR1701.py", line 3, in <module>
#     if isinstance(None, TYPES | set | float):
#                         ~~~~~~^~~~~
# TypeError: unsupported operand type(s) for |: 'tuple' and 'type'
```

---

_Comment by @charliermarsh on 2024-05-30 13:38_

Thanks - we should mark it as unsafe. We can improve the rule in the future when we support type inference.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-30 17:43_

---

_Comment by @charliermarsh on 2024-05-30 18:00_

Actually, `isinstance(None, (TYPES, set | float))` is safe... so in theory we could always rewrite to the tuple variant if we can't be certain that `|` will work?

---

_Closed by @charliermarsh on 2024-05-30 18:05_

---
