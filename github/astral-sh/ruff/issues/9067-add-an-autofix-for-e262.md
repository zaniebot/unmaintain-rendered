---
number: 9067
title: "Add an autofix for `E262`"
type: issue
state: closed
author: Skylion007
labels:
  - fixes
assignees: []
created_at: 2023-12-09T15:42:34Z
updated_at: 2023-12-09T20:18:08Z
url: https://github.com/astral-sh/ruff/issues/9067
synced_at: 2026-01-07T13:12:15-06:00
---

# Add an autofix for `E262`

---

_Issue opened by @Skylion007 on 2023-12-09 15:42_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Almost all the trivial pycodestyle e autofixes https://docs.astral.sh/ruff/rules/#error-e are implemented, but there are few that are missing that seem quite trivial to implement. 

Another trivial (and quite common one to encounter after adding manual noqas) is https://docs.astral.sh/ruff/rules/no-space-after-inline-comment/ , ie `E262`. The fix looks rather trivial to make and we should consider adding an autofix before it leaves preview.  May be a good first issue.

---

_Label `autofix` added by @charliermarsh on 2023-12-09 16:02_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-09 19:52_

---

_Referenced in [astral-sh/ruff#9075](../../astral-sh/ruff/pulls/9075.md) on 2023-12-09 19:52_

---

_Closed by @charliermarsh on 2023-12-09 20:18_

---
