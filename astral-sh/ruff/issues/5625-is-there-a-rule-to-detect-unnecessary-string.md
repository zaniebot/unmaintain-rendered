```yaml
number: 5625
title: "Is there a rule to detect unnecessary string literals in a `format` call on a string literal?"
type: issue
state: closed
author: harupy
labels: []
assignees: []
created_at: 2023-07-09T02:14:55Z
updated_at: 2023-07-09T13:21:36Z
url: https://github.com/astral-sh/ruff/issues/5625
synced_at: 2026-01-12T15:54:45Z
```

# Is there a rule to detect unnecessary string literals in a `format` call on a string literal?

---

_@harupy_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Not an issue, but a question (and a new rule idea?). I found code like this in https://github.com/mlflow/mlflow today:

```python
a = 1
"{} {}".format(a, "a")
                  ^^^ useless
```

This should be rewritten to:

```python
a = 1
"{} a".format(a)
```

Is there a rule to detect unnecessary string literals in a `format` call on a string literal?

---

_Renamed from "Is there a rule to detect useless string literals in a `format` call?" to "Is there a rule to detect useless string literals in a `format` call on a string literal?" by @harupy on 2023-07-09 02:31_

---

_Renamed from "Is there a rule to detect useless string literals in a `format` call on a string literal?" to "Is there a rule to detect unnecessary string literals in a `format` call on a string literal?" by @harupy on 2023-07-09 05:00_

---

_Comment by @harupy on 2023-07-09 13:21_

Might not be worth implementing because this is probably uncommon.

---

_Closed by @harupy on 2023-07-09 13:21_

---
