---
number: 2941
title: "Add `asyncio.ensure_future` support to `RUF006`"
type: issue
state: closed
author: zunda-arrow
labels:
  - rule
assignees: []
created_at: 2023-02-15T21:24:44Z
updated_at: 2023-02-15T23:18:13Z
url: https://github.com/astral-sh/ruff/issues/2941
synced_at: 2026-01-07T13:12:14-06:00
---

# Add `asyncio.ensure_future` support to `RUF006`

---

_Issue opened by @zunda-arrow on 2023-02-15 21:24_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

[asyncio.ensure_future](https://docs.python.org/3/library/asyncio-future.html#asyncio.ensure_future) can be garbage collected similar to `create_task`, causing the task to never finish.

From asyncio docs:
> Save a reference to the result of this function, to avoid a task disappearing mid-execution.


---

_Label `rule` added by @charliermarsh on 2023-02-15 21:55_

---

_Referenced in [astral-sh/ruff#2943](../../astral-sh/ruff/pulls/2943.md) on 2023-02-15 21:58_

---

_Closed by @charliermarsh on 2023-02-15 23:18_

---
