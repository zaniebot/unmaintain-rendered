---
number: 9615
title: Support for allowing other context managers in SIM115
type: issue
state: open
author: AbdealiLoKo
labels:
  - configuration
assignees: []
created_at: 2024-01-22T16:01:12Z
updated_at: 2024-01-24T02:58:37Z
url: https://github.com/astral-sh/ruff/issues/9615
synced_at: 2026-01-10T01:22:49Z
---

# Support for allowing other context managers in SIM115

---

_Issue opened by @AbdealiLoKo on 2024-01-22 16:01_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I recently came across the SIM115 rule which encourages the use of context manager for `open()`

I use pyfilesystem2 - which uses [fs.openbin](https://docs.pyfilesystem.org/en/latest/reference/osfs.html?highlight=openbin#fs.osfs.OSFS.openbin)

Is there any option in ruff to provide the functions which should be recommended as context managers ?
I learned that logger related rules have a `logger-objects` here: https://github.com/astral-sh/ruff/issues/6764
Is there a similar `context-manager-objects` setting or something similar to encourage my developers to always use the `openbin` function with a context manager ?


---

_Label `configuration` added by @charliermarsh on 2024-01-24 02:58_

---
