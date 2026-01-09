---
number: 2772
title: Incorrect double reverse in sorted rule C413
type: issue
state: closed
author: Skylion007
labels:
  - bug
assignees: []
created_at: 2023-02-11T17:23:14Z
updated_at: 2023-02-12T15:56:08Z
url: https://github.com/astral-sh/ruff/issues/2772
synced_at: 2026-01-07T13:12:14-06:00
---

# Incorrect double reverse in sorted rule C413

---

_Issue opened by @Skylion007 on 2023-02-11 17:23_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

The comprehension fixits assume the user was unaware of the reverse=True keyword. This not always the case and it can be caused by an accidental double apply or an unnecessary attempt to ensure sorted is a stable sort (it is in recent Python versions). The fixits generated here are wrong

```python
b = range(10)
c = reversed(sorted(b, reverse=True))
```
the fixit suggests:
```python
b = range(10)
c = sorted(b, reverse=True)
```
which obviously wrong. The proper fixit would be:
```python
b = range(10)
c = sorted(b)
```

Version is ruff 0.0.244. 
Command is `ruff --select C example.py --fix`

---

_Label `bug` added by @charliermarsh on 2023-02-11 17:25_

---

_Comment by @sbrugman on 2023-02-12 08:58_

Working on a fix

---

_Referenced in [astral-sh/ruff#2804](../../astral-sh/ruff/pulls/2804.md) on 2023-02-12 10:16_

---

_Closed by @charliermarsh on 2023-02-12 15:56_

---
