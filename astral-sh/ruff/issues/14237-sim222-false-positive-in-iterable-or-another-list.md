---
number: 14237
title: "SIM222 false positive in `[*iterable] or [another list]`"
type: issue
state: closed
author: rdaysky
labels:
  - bug
assignees: []
created_at: 2024-11-10T01:56:38Z
updated_at: 2024-11-11T20:23:35Z
url: https://github.com/astral-sh/ruff/issues/14237
synced_at: 2026-01-10T01:22:54Z
---

# SIM222 false positive in `[*iterable] or [another list]`

---

_Issue opened by @rdaysky on 2024-11-10 01:56_

Suppose I want to run some code for each element in an iterable, but if there are none, run once for the value of None. However, the natural code
```
def f(some_iterable):
    for x in [*some_iterable] or [None]:
        do_something(x)
```
fails SIM222, which wants to remove the `or [None]` part, presumably thinking the list will always be truthy. Similarly to https://github.com/astral-sh/ruff/issues/9479, the rule needs to take into account that a list/set/dict literal containing only *-elements can still end up empty.

---

_Renamed from "SIM222 false positive in `if [*iterable] or [another list]`" to "SIM222 false positive in `[*iterable] or [another list]`" by @rdaysky on 2024-11-10 01:57_

---

_Comment by @InSyncWithFoo on 2024-11-10 04:34_

The same holds for other literal iterables as well ([playground](https://play.ruff.rs/7e566313-3d07-472e-b4f3-ef99c9a43c55)):

```py
[*a] or ...
[*a, *b] or ...
{*a} or ...
{*a, *b} or ...
(*a,) or ...
(*a, *b) or ...
{**a} or ...
{**a, **b} or ...
```

I'll work on a PR.

---

_Label `bug` added by @MichaReiser on 2024-11-10 09:24_

---

_Referenced in [astral-sh/ruff#14263](../../astral-sh/ruff/pulls/14263.md) on 2024-11-11 05:15_

---

_Closed by @charliermarsh on 2024-11-11 20:23_

---

_Closed by @charliermarsh on 2024-11-11 20:23_

---
