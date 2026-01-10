---
number: 17465
title: "[red-knot] avoid divergent iteration due to unbounded union growth"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2025-04-18T17:28:07Z
updated_at: 2025-04-18T22:08:59Z
url: https://github.com/astral-sh/ruff/issues/17465
synced_at: 2026-01-10T01:22:58Z
---

# [red-knot] avoid divergent iteration due to unbounded union growth

---

_Issue opened by @carljm on 2025-04-18 17:28_

In this code sample (provided by @skshetry in https://github.com/astral-sh/ruff/issues/17457#issuecomment-2815848955), we enter divergent fixpoint iteration and end up panicking with "too many iterations":

```py
class Klass:
    def __init__(self) -> None:
        self.attr = 1

    def copy(self, other: "Klass"):
        self.attr = other.attr or self.attr
```

The issue is that we infer a type of `Unknown & ~AlwaysFalsy`, which we then union with itself on each subsequent iteration, but we fail to simplify this union, so it just grows on each iteration.

There should be multiple parts to the fix here. One is that we should be able to simplify `(Unknown & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy)` to just `Unknown & ~AlwaysFalsy`.

The other is that we should have a reliable backstop, since our union and intersection simplification will never be perfect. If any union or intersection grows over a certain size limit, we need to just best-effort reduce it to the nearest enclosing "join" type, or just `object`.

---

_Label `red-knot` added by @carljm on 2025-04-18 17:28_

---

_Added to milestone `Red Knot Alpha` by @carljm on 2025-04-18 17:28_

---

_Referenced in [astral-sh/ruff#17457](../../astral-sh/ruff/issues/17457.md) on 2025-04-18 17:28_

---

_Assigned to @carljm by @carljm on 2025-04-18 17:30_

---

_Referenced in [astral-sh/ruff#17467](../../astral-sh/ruff/pulls/17467.md) on 2025-04-18 18:11_

---

_Comment by @carljm on 2025-04-18 18:13_

https://github.com/astral-sh/ruff/pull/17467 will fix this case.

I'm inclined to not add the generalized backstop yet, because it will paper over cases of this and turn them from "panic" to "slow and imprecise but doesn't panic". The latter is harder to detect and may cause us to miss problems. We probably want it eventually, but I'd rather not have it yet.

---

_Closed by @carljm on 2025-04-18 22:08_

---

_Closed by @carljm on 2025-04-18 22:08_

---
