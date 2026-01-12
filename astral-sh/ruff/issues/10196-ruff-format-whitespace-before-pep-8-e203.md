```yaml
number: 10196
title: "ruff format: whitespace before `:` (PEP 8: E203)"
type: issue
state: closed
author: Zerotask
labels: []
assignees: []
created_at: 2024-03-02T14:59:01Z
updated_at: 2024-03-03T00:05:34Z
url: https://github.com/astral-sh/ruff/issues/10196
synced_at: 2026-01-12T15:54:50Z
```

# ruff format: whitespace before `:` (PEP 8: E203)

---

_@Zerotask_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

```python
field_types_1 = field_types[len(field_types) // 2 :]
``` 

PyCharm marks it as: `PEP 8: E203 whitespace before ':'`.
So the expected code looks like:
```python
field_types_1 = field_types[len(field_types) // 2:]
``` 

If I run ruff format / auto-format from the ruff plugin, it adds the whitespace again.

---

ruff 0.3.0 with preview enabled

---

_Comment by @57an on 2024-03-02 17:44_

This is black behaviour. You can add E203 in Ignored Errors list for PEP 8 coding style violetion check.
See Slices section [here](https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html) - "Since E203 is not PEP 8 compliant, you should tell Flake8 to ignore these warnings".

---

_Comment by @charliermarsh on 2024-03-03 00:05_

Yeah, I believe this is intentional as `E203` is in conflict with PEP 8. (Ruff's implementation of `E203` does not flag this.)

---

_Closed by @charliermarsh on 2024-03-03 00:05_

---
