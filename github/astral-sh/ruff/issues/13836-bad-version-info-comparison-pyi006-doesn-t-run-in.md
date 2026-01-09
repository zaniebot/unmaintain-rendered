---
number: 13836
title: "bad-version-info-comparison (PYI006) doesn't run in `.py` files"
type: issue
state: closed
author: Avasam
labels:
  - rule
  - help wanted
  - accepted
assignees: []
created_at: 2024-10-20T21:25:30Z
updated_at: 2024-11-03T11:47:37Z
url: https://github.com/astral-sh/ruff/issues/13836
synced_at: 2026-01-07T13:12:16-06:00
---

# bad-version-info-comparison (PYI006) doesn't run in `.py` files

---

_Issue opened by @Avasam on 2024-10-20 21:25_

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

This rule could've caught an issue in pywin32 (https://github.com/mhammond/pywin32/pull/2416). I think it's a valid rule to enforce on runtime files to prevent incorrect expectations.

MRE:
```python
if sys.version_info > (3, 10):  # PYI006 only shows here in .pyi files
    print("Do something on Python 3.11")
```

Command: `ruff check --select=PYI`
Version: ruff 0.7.0


---

_Referenced in [mhammond/pywin32#2416](../../mhammond/pywin32/pulls/2416.md) on 2024-10-20 21:27_

---

_Comment by @AlexWaygood on 2024-10-20 21:32_

This makes sense to me.

---

_Label `accepted` added by @AlexWaygood on 2024-10-20 21:32_

---

_Label `rule` added by @dhruvmanila on 2024-10-30 03:45_

---

_Label `help wanted` added by @dhruvmanila on 2024-10-30 03:45_

---

_Referenced in [astral-sh/ruff#14059](../../astral-sh/ruff/pulls/14059.md) on 2024-11-02 23:25_

---

_Closed by @MichaReiser on 2024-11-03 11:47_

---
