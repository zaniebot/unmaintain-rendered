```yaml
number: 12431
title: 0.5.4 Windows artifact flagged on Chromium
type: issue
state: closed
author: AlexBlandin
labels: []
assignees: []
created_at: 2024-07-21T14:55:07Z
updated_at: 2024-07-23T12:28:01Z
url: https://github.com/astral-sh/ruff/issues/12431
synced_at: 2026-01-10T11:09:54Z
```

# 0.5.4 Windows artifact flagged on Chromium

---

_Issue opened by @AlexBlandin on 2024-07-21 14:55_

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

Downloading [0.5.4](https://github.com/astral-sh/ruff/releases/tag/0.5.4)'s [`ruff-x86_64-pc-windows-msvc.zip`](https://github.com/astral-sh/ruff/releases/download/0.5.4/ruff-x86_64-pc-windows-msvc.zip) gets flagged as a virus (presumably false-positive) in Chromium browsers (Vivaldi 6.8.3381.48). Unfortunately, don't have the bandwidth to do any further investigation on my end.


---

_Comment by @charliermarsh on 2024-07-21 17:31_

Unfortunately there isn't much we can do here typically.

---

_Closed by @charliermarsh on 2024-07-21 17:31_

---

_Comment by @AlexBlandin on 2024-07-23 11:54_

Thankfully, it seems to have passed, and now isn't flagged. Wish there was more we could do to avoid this in general, but alas.

---

_Comment by @charliermarsh on 2024-07-23 12:28_

Totally agree.

---
