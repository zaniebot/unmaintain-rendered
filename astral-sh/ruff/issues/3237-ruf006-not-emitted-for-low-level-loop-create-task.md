```yaml
number: 3237
title: "RUF006 not emitted for low-level `loop.create_task`"
type: issue
state: closed
author: layday
labels:
  - bug
  - type-inference
assignees: []
created_at: 2023-02-26T10:15:15Z
updated_at: 2023-09-21T04:37:39Z
url: https://github.com/astral-sh/ruff/issues/3237
synced_at: 2026-01-12T15:54:43Z
```

# RUF006 not emitted for low-level `loop.create_task`

---

_@layday_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

AFAIK, the same pitfall applies to the lower level `loop.create_task`:

```py
import asyncio


async def foo():
    asyncio.create_task(bar())  # Warning here
    loop = asyncio.get_running_loop()
    loop.create_task(bar())  # No warning here

async def bar():
    pass

```

It would be great if ruff could support this too in some way.

Ruff version: 0.0.252

---

_Label `bug` added by @charliermarsh on 2023-02-26 23:27_

---

_Comment by @charliermarsh on 2023-02-26 23:28_

This requires more static analysis than we're currently capable of doing, but I'd like to support these kinds of checks so I'm leaving this open.

---

_Label `type-inference` added by @charliermarsh on 2023-02-27 21:49_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-21 04:26_

---

_Closed by @charliermarsh on 2023-09-21 04:37_

---
