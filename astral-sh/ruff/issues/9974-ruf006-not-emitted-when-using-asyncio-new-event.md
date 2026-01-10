```yaml
number: 9974
title: RUF006 not emitted when using asyncio.new_event_loop
type: issue
state: closed
author: tyilo
labels:
  - bug
  - good first issue
assignees: []
created_at: 2024-02-13T15:44:33Z
updated_at: 2024-02-13T18:28:07Z
url: https://github.com/astral-sh/ruff/issues/9974
synced_at: 2026-01-10T11:09:52Z
```

# RUF006 not emitted when using asyncio.new_event_loop

---

_Issue opened by @tyilo on 2024-02-13 15:44_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Running `pipx run ruff test.py --select 'RUF006'` on the following `test.py` file, gives no errors:
```python
import asyncio


async def main():
    pass


loop = asyncio.new_event_loop()
loop.create_task(main())
loop.run_forever()
```

I'm using ruff 0.2.1.

---

_Label `bug` added by @zanieb on 2024-02-13 16:08_

---

_Comment by @zanieb on 2024-02-13 16:08_

Thanks for the report!

---

_Label `good first issue` added by @zanieb on 2024-02-13 16:08_

---

_Closed by @charliermarsh on 2024-02-13 18:28_

---
