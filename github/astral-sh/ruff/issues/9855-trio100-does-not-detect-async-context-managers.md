---
number: 9855
title: "`TRIO100` does not detect async context managers"
type: issue
state: closed
author: CoolCat467
labels:
  - bug
assignees: []
created_at: 2024-02-06T04:25:37Z
updated_at: 2024-02-06T17:04:25Z
url: https://github.com/astral-sh/ruff/issues/9855
synced_at: 2026-01-07T13:12:15-06:00
---

# `TRIO100` does not detect async context managers

---

_Issue opened by @CoolCat467 on 2024-02-06 04:25_

Noticed this in my work on https://github.com/python-trio/trio/pull/2947, it appears that `TRIO100` does not detect async context managers.

`minimal.py`:
```python3
import trio

async def test_reentry_doesnt_deadlock() -> None:
    # Regression test for issue noticed in GH-2827
    # The failure mode is to hang the whole test suite, unfortunately.
    # XXX consider running this in a subprocess with a timeout, if it comes up again!

    async def child() -> None:
        while True:
            await trio.to_thread.run_sync(trio.from_thread.run, trio.sleep, 0, abandon_on_cancel=False)

    with trio.move_on_after(2):
        async with trio.open_nursery() as nursery:
            for _ in range(4):
                nursery.start_soon(child)
```

```console
> ruff check minimal.py --isolated --select TRIO
minimal.py:12:5: TRIO100 A `with trio.move_on_after(...):` context does not contain any `await` statements. This makes it pointless, as the timeout can only be triggered by a checkpoint.
```

```console
> ruff --version
ruff 0.2.1
```

---

_Label `bug` added by @charliermarsh on 2024-02-06 04:31_

---

_Comment by @charliermarsh on 2024-02-06 04:31_

Thanks!

---

_Referenced in [python-trio/trio#2947](../../python-trio/trio/pulls/2947.md) on 2024-02-06 04:48_

---

_Referenced in [astral-sh/ruff#9859](../../astral-sh/ruff/pulls/9859.md) on 2024-02-06 16:46_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-06 16:46_

---

_Closed by @charliermarsh on 2024-02-06 17:04_

---

_Referenced in [astral-sh/ruff#9934](../../astral-sh/ruff/issues/9934.md) on 2024-02-11 23:28_

---
