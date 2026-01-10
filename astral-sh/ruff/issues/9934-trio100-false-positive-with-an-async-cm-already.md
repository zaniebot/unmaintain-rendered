```yaml
number: 9934
title: "TRIO100 false positive with an async CM (already fixed in `flake8-trio` - ruff is out of date)"
type: issue
state: closed
author: mikenerone
labels:
  - bug
assignees: []
created_at: 2024-02-11T19:37:13Z
updated_at: 2024-02-12T00:36:02Z
url: https://github.com/astral-sh/ruff/issues/9934
synced_at: 2026-01-10T11:09:52Z
```

# TRIO100 false positive with an async CM (already fixed in `flake8-trio` - ruff is out of date)

---

_Issue opened by @mikenerone on 2024-02-11 19:37_

Ruff 0.2.1

The following snippet illustrates a TRIO100 false positive when the scope contains an async context manager. Note that this bug is already fixed in `flake8-trio` (in Zac-HD/flake8-trio/pull/176 I _think_). It's probably worth checking whether ruff needs to port other more recent fixes, as well.

```python
from collections.abc import AsyncGenerator
from contextlib import asynccontextmanager

import trio

@asynccontextmanager
async def a_cm_can_start_a_nursery() -> AsyncGenerator[None]:
    async with trio.open_nursery() as nursery:
        nursery.start_soon(trio.sleep_forever)
        yield

async def main() -> None:
    # ↓↓↓↓↓ TRIO100 A `with trio.CancelScope(...):` context does not contain any `await` statements. This makes it pointless, as the timeout can only be triggered by a checkpoint.
    with trio.CancelScope() as cancel_scope:
        async with a_cm_can_start_a_nursery():
            try:
                pass # Pretend there's some real logic.
            finally:
                # Body exiting. For all I know, the CM has nursery like this one does. If so, I need it cancelled.
                cancel_scope.cancel()

trio.run(main)
```
The above is a valid case for a cancel scope with no explicit awaits in the body. Without the cancel(), this hangs forever instead of exiting.

Note: if I put an `async for` in the `try:`, it's even more obvious that this yields, but TRIO100 still triggers.

---

_Label `bug` added by @charliermarsh on 2024-02-11 19:39_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-11 19:39_

---

_Comment by @charliermarsh on 2024-02-11 20:59_

Thanks, will fix this tonight.

---

_Comment by @mikenerone on 2024-02-11 23:28_

@charliermarsh I missed #9855. It looks like you may have already fixed this in main.

---

_Comment by @charliermarsh on 2024-02-12 00:35_

Wow, yeah, I fixed it _this week_ and didn't even remember!

---

_Closed by @charliermarsh on 2024-02-12 00:35_

---

_Comment by @charliermarsh on 2024-02-12 00:36_

Will be included in the next release.

---
