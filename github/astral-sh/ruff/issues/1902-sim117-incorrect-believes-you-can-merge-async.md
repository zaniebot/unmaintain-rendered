---
number: 1902
title: "`SIM117` incorrect believes you can merge `async with` and `with`"
type: issue
state: closed
author: thomasdesr
labels:
  - bug
assignees: []
created_at: 2023-01-16T02:16:37Z
updated_at: 2023-01-16T03:34:22Z
url: https://github.com/astral-sh/ruff/issues/1902
synced_at: 2026-01-07T13:12:14-06:00
---

# `SIM117` incorrect believes you can merge `async with` and `with`

---

_Issue opened by @thomasdesr on 2023-01-16 02:16_

SIM117 should:
* Not trigger when nesting a context manager within an async context manager since you can't generally merge an async context manager with a sync context manager.
* Should trigger when nesting two async context managers (edit: actually maybe should be a separate lint?)

Example of permutations and their results as of `0.0.222`:

```python
import asyncio
from contextlib import asynccontextmanager, contextmanager


@asynccontextmanager
async def async_ctx_mgr():
    yield "async"


@contextmanager
def sync_ctx_mgr():
    yield "sync"


async def main():
    # True Positive, correctly suggests to minimize
    with sync_ctx_mgr() as first:
        with sync_ctx_mgr() as second:
            print("sync sync", first, second)

    # False positive, incorrectly suggests to minimize
    async with async_ctx_mgr() as first:
        with sync_ctx_mgr() as second:
            print("async sync", first, second)

    # True negative, correctly doesn't flag as an issue
    with sync_ctx_mgr() as first:
        async with async_ctx_mgr() as second:
            print("sync async", first, second)

    # False negative, incorrectly fails to suggest to minimize
    async with async_ctx_mgr() as first:
        async with async_ctx_mgr() as second:
            print("async async", first, second)


if __name__ == "__main__":
    asyncio.run(main())
```


Output from `ruff` on ^
```
$ ruff --isolated --select SIM ./SIM117-repro.py
SIM117-repro.py:16:5: SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
SIM117-repro.py:20:5: SIM117 Use a single `with` statement with multiple contexts instead of nested `with` statements
Found 2 error(s).
```

```
$ ruff --version
ruff 0.0.222
```

Note: `flake8-simplify` seems to do the right thing here even though their tests are missing coverage of this case.

Thanks as always!

---

_Renamed from "SIM117 incorrect believes you can merge `async with` and `with`" to "`SIM117` incorrect believes you can merge `async with` and `with`" by @thomasdesr on 2023-01-16 02:18_

---

_Label `bug` added by @charliermarsh on 2023-01-16 02:19_

---

_Comment by @charliermarsh on 2023-01-16 02:19_

Good call.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-16 02:19_

---

_Comment by @charliermarsh on 2023-01-16 02:20_

I'll try to fix this tonight :)

---

_Comment by @thomasdesr on 2023-01-16 02:20_

Thanks a bunch <3

---

_Referenced in [astral-sh/ruff#1903](../../astral-sh/ruff/pulls/1903.md) on 2023-01-16 02:33_

---

_Closed by @charliermarsh on 2023-01-16 02:42_

---

_Comment by @thomasdesr on 2023-01-16 03:05_

So fast!

Re-ran with a main build and the sync context manager case looks fixed. Thanks ❤️ 

---

_Comment by @charliermarsh on 2023-01-16 03:08_

Awesome :) I opted to just turn it off entirely in the async case. I think it's safe to combine multiple async context managers, but honestly I've done so little async Python that I'd have to check.


---

_Comment by @charliermarsh on 2023-01-16 03:08_

(Also I'm going to cut a new build tonight, so this will be out in a bit.)

---

_Comment by @thomasdesr on 2023-01-16 03:34_

Sounds good! Thanks so much :D

I also believe its fine to merge async context managers, but double checking sgtm xD

---

_Referenced in [astral-sh/ruff#3025](../../astral-sh/ruff/issues/3025.md) on 2023-02-19 13:49_

---

_Referenced in [astral-sh/ruff#5022](../../astral-sh/ruff/pulls/5022.md) on 2023-06-12 10:34_

---
