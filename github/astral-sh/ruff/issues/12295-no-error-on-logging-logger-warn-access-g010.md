---
number: 12295
title: "No error on `logging.Logger.warn` access [G010]"
type: issue
state: open
author: rominf
labels:
  - bug
  - type-inference
assignees: []
created_at: 2024-07-12T08:32:29Z
updated_at: 2024-07-12T12:50:05Z
url: https://github.com/astral-sh/ruff/issues/12295
synced_at: 2026-01-07T13:12:15-06:00
---

# No error on `logging.Logger.warn` access [G010]

---

_Issue opened by @rominf on 2024-07-12 08:32_

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

Before creating this issue I searched for:
```
G010
logger warn
logger.warn
```
but could not find previous issue or PR.

Minimal code snippet (`ruff_G010_no_call.py`) that reproduces the bug:
```
"""No G010 on logging.Logger.warn access."""
import logging

logger = logging.getLogger()
warn = logger.warn
warn("Hello, world!")
```

I invoked `ruff` with:
```
$ ruff check ruff_G010_no_call.py --isolated --select G010 --target-version py313
$ ruff check ruff_G010_no_call.py --isolated --select ALL --target-version py313
```
but got no errors.

I expected to get `G010` (https://docs.astral.sh/ruff/rules/logging-warn/) on line `warn = logger.warn`. The code like this is quite wide-spread, here are the results of GitHub search that show code most likely to be invalid in Python 3.13, but that won't be detected with the current version of ruff: https://github.com/search?q=%2F%28%3F-i%29%28logger%7Clogging%29.warn%24%2F+language%3APython&type=code

I installed `ruff` from PyPI first, then from `https://github.com/astral-sh/ruff/archive/refs/heads/main.zip`:
```
$ python3 --version 
Python 3.13.0b3
$ ruff --version
ruff 0.5.1
```

As a side note, current stable version of `mypy` does not find this issue either. `mypy` installed from `master` can find this issue though.

---

_Referenced in [celery/kombu#2058](../../celery/kombu/pulls/2058.md) on 2024-07-12 09:24_

---

_Label `type-inference` added by @charliermarsh on 2024-07-12 12:35_

---

_Comment by @charliermarsh on 2024-07-12 12:49_

This is known -- we use a heuristic to detect if a call is a "logging call", and we don't trace name symbols like that. I can't find an obvious issue to link this to though, so I'll leave it as-is.

---

_Label `bug` added by @charliermarsh on 2024-07-12 12:49_

---

_Comment by @charliermarsh on 2024-07-12 12:50_

(`logger.warn("...")` should work, but I think you picked that up already :))

---

_Referenced in [astral-sh/ruff#17219](../../astral-sh/ruff/issues/17219.md) on 2025-04-05 08:46_

---
