---
number: 9935
title: "TRIO115 false positive with with `sleep(var)` where `var` starts as `0`"
type: issue
state: closed
author: mikenerone
labels:
  - bug
assignees: []
created_at: 2024-02-11T19:50:03Z
updated_at: 2024-03-27T23:32:45Z
url: https://github.com/astral-sh/ruff/issues/9935
synced_at: 2026-01-07T13:12:15-06:00
---

# TRIO115 false positive with with `sleep(var)` where `var` starts as `0`

---

_Issue opened by @mikenerone on 2024-02-11 19:50_

Ruff 0.2.1

The following snippet illustrates a TRIO115 false positive when `sleep()` is passed a variable whose initial value was `0`, but changes later in a loop construct. Similar to #9934, this is already fixed in `flake8-trio`. This seems to be strongly indicating that recent fixes to `flake8-trio` need to be ported in general.

```python
import trio

async def main() -> None:
    sleep = 0
    for _ in range(2):
        # ↓↓↓↓↓ TRIO115 [*] Use `trio.lowlevel.checkpoint()` instead of `trio.sleep(0)`
        await trio.sleep(sleep)
        sleep = 10

trio.run(main)
```

---

_Referenced in [python-trio/trio#2947](../../python-trio/trio/pulls/2947.md) on 2024-02-11 20:21_

---

_Comment by @charliermarsh on 2024-02-11 20:50_

Thanks. I don't think this has been "fixed" in `flake8-trio` -- rather, `flake8-trio` _never_ flags with a non-constant, it only flags if you pass exactly `0`. You can see the source here: https://github.com/Zac-HD/flake8-trio/blob/a652441fb3a9a92f14da3542bb2a0a1ba8f03f87/flake8_trio/visitors/visitors.py#L219.

---

_Comment by @charliermarsh on 2024-02-11 20:51_

We could consider doing the same, or just check if the variable is ever re-bound to something else.

---

_Label `bug` added by @charliermarsh on 2024-02-11 20:51_

---

_Renamed from "TRIO115 false positive with with `sleep(var)` where `var` starts as `0` (already fixed in `flake8-trio` - ruff is out of date)" to "TRIO115 false positive with with `sleep(var)` where `var` starts as `0`" by @charliermarsh on 2024-02-11 20:51_

---

_Comment by @mikenerone on 2024-02-11 22:10_

> Thanks. I don't think this has been "fixed" in `flake8-trio` -- rather, `flake8-trio` _never_ flags with a non-constant, it only flags if you pass exactly `0`.

> We could consider doing the same, or just check if the variable is ever re-bound to something else.

@charliermarsh Oh, I see - you're correct. Not flagging on a non-constant sounds reasonable to me. With a variable, even _if_ not re-bound, it seems like the developer is signaling that it's considered changeable in at least some sense.



---

_Comment by @charliermarsh on 2024-02-11 23:04_

Agreed.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-12 00:49_

---

_Comment by @jakkdl on 2024-02-12 10:10_

flake8-trio dev here:
the "constant" in the source code is just how the AST labels literals, flake8-trio doesn't track the values of any variables (and has no straightforward way of doing so)

---

_Referenced in [astral-sh/ruff#10376](../../astral-sh/ruff/pulls/10376.md) on 2024-03-13 04:30_

---

_Comment by @robincaloudis on 2024-03-27 22:27_

As https://github.com/astral-sh/ruff/pull/10376 is merged, it looks like this issue could be closed.

---

_Closed by @charliermarsh on 2024-03-27 23:32_

---
