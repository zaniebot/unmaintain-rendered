```yaml
number: 690
title: better narrowing from conditional terminals and NoReturn calls
type: issue
state: open
author: carljm
labels:
  - narrowing
assignees: []
created_at: 2025-06-21T19:57:34Z
updated_at: 2026-01-09T15:29:51Z
url: https://github.com/astral-sh/ty/issues/690
synced_at: 2026-01-12T15:54:23Z
```

# better narrowing from conditional terminals and NoReturn calls

---

_@carljm_

Currently, a narrowing constraint is just an expression. We don't support boolean connectives of narrowing constraints -- we just track an implicit AND of multiple narrowing constraints per definition. Since we have no way to represent an `OR` of narrowing constraints, when we merge control flow paths we just discard any narrowing constraint that is present on only one path.

This means that we fail to preserve some valid narrowing information in a case like this one:

```py
class A: pass
class B: pass
class C: pass

def _(x: A | B | C):
    if isinstance(x, A):
        pass
    elif isinstance(x, B):
        pass
    else:
        return

    reveal_type(x)  # should be `A | (B & ~A)`, we currently reveal `A | B | C`
```

https://play.ty.dev/21e55e75-9ca4-46ca-ac62-24f1a6abecb1

This also impacts narrowing from `NoReturn` calls (e.g. `sys.exit`) in a conditional branch.

This problem also blocks progress on #626, since the fix for that issue requires "breaking up" a test expression like `if isinstance(x, A) or isinstance(x, B)` into two separate narrowing constraints (where today it is just one narrowing constraint, with the `or` handled in `narrow.rs`), and our inability to represent an `OR` of two separate narrowing constraints means that a fix for #626 regresses our current understanding of narrowing from such expressions

One way to fix this might be to reuse our TDD implementation for reachability constraints, and use it also for narrowing constraints. This could even simplify the use-def map, since it would mean we'd only need to track a single narrowing constraint per definition (it could be an AND of multiple other constraints).

An even deeper fix could be to treat every new narrowing of a definition as a new "definition" of that symbol, and just use visibility constraints to decide which such "definitions" are visible. This approach could also fix #685.

---

_Label `narrowing` added by @carljm on 2025-06-21 19:57_

---

_Renamed from "support better preservation of narrowing constraints in control flow joins" to "better preserve narrowing constraints in control flow joins" by @carljm on 2025-06-21 19:58_

---

_Comment by @carljm on 2025-06-21 19:59_

cc @TomerBin re #626 

---

_Added to milestone `GA` by @carljm on 2025-07-23 22:55_

---

_Removed from milestone `GA` by @carljm on 2025-07-25 14:37_

---

_Added to milestone `Beta` by @carljm on 2025-07-25 14:37_

---

_Removed from milestone `Beta` by @carljm on 2025-08-15 15:27_

---

_Added to milestone `GA` by @carljm on 2025-08-15 15:27_

---

_Renamed from "better preserve narrowing constraints in control flow joins" to "better narrowing from conditional terminals and NoReturn calls" by @carljm on 2025-12-16 22:24_

---

_Removed from milestone `Stable` by @carljm on 2025-12-20 00:35_

---

_Added to milestone `M1` by @carljm on 2025-12-20 00:35_

---

_Assigned to @oconnor663 by @oconnor663 on 2026-01-09 15:29_

---
