```yaml
number: 3515
title: PIE810 autofixit bug - removes parentheses
type: issue
state: closed
author: Skylion007
labels:
  - question
assignees: []
created_at: 2023-03-14T17:09:24Z
updated_at: 2023-03-14T19:15:44Z
url: https://github.com/astral-sh/ruff/issues/3515
synced_at: 2026-01-12T15:54:43Z
```

# PIE810 autofixit bug - removes parentheses

---

_@Skylion007_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

flake8-PIE810 can accidentally remove parentheses and change the meetings of expressions. See below for a real world error.

Original Code
```python
a = "b"

a.startswith("c") or a.startswith("d") or (a.startswith('b') and not a.endswith('f'))
```

Autofixed code
```python
a = "b"

a.startswith(("c", "d")) or a.startswith("b") and not a.endswith('f')
```

The two if statements are not equivalent due to missing parentheses around the "and" statement. This bug may also affect the isinstance merge checks in flake8-SIM as well.

---

_Comment by @charliermarsh on 2023-03-14 17:14_

Due to precedence, I believe those statements are the same? E.g., `a = "c"` returns the same thing in both cases.

---

_Label `question` added by @charliermarsh on 2023-03-14 17:15_

---

_Closed by @Skylion007 on 2023-03-14 19:15_

---
