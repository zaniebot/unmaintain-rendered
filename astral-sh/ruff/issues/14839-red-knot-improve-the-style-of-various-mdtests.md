---
number: 14839
title: "[red-knot] Improve the style of various mdtests"
type: issue
state: closed
author: AlexWaygood
labels:
  - help wanted
  - testing
  - ty
assignees: []
created_at: 2024-12-08T17:21:26Z
updated_at: 2024-12-10T13:05:53Z
url: https://github.com/astral-sh/ruff/issues/14839
synced_at: 2026-01-10T01:22:55Z
---

# [red-knot] Improve the style of various mdtests

---

_Issue opened by @AlexWaygood on 2024-12-08 17:21_

We have a lot of [mdtests](https://github.com/astral-sh/ruff/tree/main/crates/red_knot_test) in red-knot that have this pattern in them to create instance types:

```py
def bool_instance() -> bool:
    return True

x = bool_instance()

def int_instance() -> int:
    return 42

y = int_instance()

# do some reveal_type calls or whatever in the test now with `x` and `y`
```

Now that https://github.com/astral-sh/ruff/pull/14802 has landed, however, we don't need nearly as much boilerplate in our tests. Instead, we can do something much more elegant:

```py
def f(x: bool, y: int):
    ...
    # do some reveal_type calls or whatever with `x` and `y`
```

We should go through all the Python snippets in https://github.com/astral-sh/ruff/tree/main/crates/red_knot_python_semantic/resources/mdtest to reduce the boilerplate in our mdtests.

---

_Label `help wanted` added by @AlexWaygood on 2024-12-08 17:21_

---

_Label `red-knot` added by @AlexWaygood on 2024-12-08 17:21_

---

_Label `testing` added by @AlexWaygood on 2024-12-08 17:21_

---

_Referenced in [astral-sh/ruff#14858](../../astral-sh/ruff/pulls/14858.md) on 2024-12-09 12:36_

---

_Comment by @InSyncWithFoo on 2024-12-09 15:37_

I'll take this. PR coming tomorrow.

---

_Assigned to @InSyncWithFoo by @AlexWaygood on 2024-12-09 15:43_

---

_Referenced in [astral-sh/ruff#14884](../../astral-sh/ruff/pulls/14884.md) on 2024-12-10 00:42_

---

_Closed by @AlexWaygood on 2024-12-10 13:05_

---
