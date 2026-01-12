```yaml
number: 1154
title: right hand of operator incorrectly takes precedence
type: issue
state: open
author: KotlinIsland
labels: []
assignees: []
created_at: 2025-09-09T08:56:33Z
updated_at: 2025-09-10T00:30:54Z
url: https://github.com/astral-sh/ty/issues/1154
synced_at: 2026-01-12T15:54:24Z
```

# right hand of operator incorrectly takes precedence

---

_@KotlinIsland_

### Summary

```py
from __future__ import annotations

class A:
    def __or__(self, other: A) -> A:
        return self

    def __ror__(self, other: A) -> B:
        return B()

class B(A):
    def __ror__(self, other: A) -> B:
        return B()

class C(A):
    pass
    
def f(
    a: A = C(),
    b: B = B(),
):
    x = a | b  # ty: B, runtime: C

f()
```

this unsafety is also discussed in this issue:
- #630


---

_Renamed from "regression: right hand of operator incorrectly takes precedence" to "right hand of operator incorrectly takes precedence" by @KotlinIsland on 2025-09-09 09:29_

---

_Comment by @carljm on 2025-09-09 16:54_

Pyright gives `str` here (though it's not clear to me what logic it uses to reach that conclusion); mypy and ty both give `int`. Unlike the case in #630, mypy does not give an "unsafe overlapping dunder definitions" diagnostic here.

This seems sufficiently similar to #630 that I'm not sure it makes sense to have two separate issues open?

---

_Closed by @carljm on 2025-09-09 16:54_

---

_Comment by @KotlinIsland on 2025-09-09 23:24_

pyright gives`str` because it doesn't special case the right-hand subtype rule like mypy (and seemingly ty) does. it's invalid to take this rule into account because you don't actually know if the right is a subclass of the left, as demonstrated in the op

---

_Comment by @KotlinIsland on 2025-09-09 23:55_

@carljm these are two separate issues, this issue can be reproduced with completely valid operator definitions, i've updated the OP to demonstrate

the issue is that the right hand subtype rule should only be applied when the **exact** types of the values are known, if it's potentially a subtype then it's unsound to do this specialisation

if we were to introduce `Exact` types, like "use-site final", or an equal lower and upper bound on a type, then we could perform this safely

---

_Comment by @carljm on 2025-09-10 00:18_

Ok -- and the idea is that we can achieve soundness here by a) only applying the RHS subtype precedence rule if we know it applies, due to finality of the LHS, and b) implementing the warning in #630, so we would warn in any case where not applying the RHS subtype precedence rule could lead to unsound results.

Don't have time to work through all the cases in detail right now, but I think this makes sense.

---

_Reopened by @carljm on 2025-09-10 00:18_

---

_Comment by @KotlinIsland on 2025-09-10 00:30_

thank you 🙂

i agree with your points. although for completeness, we should say "the lower bound of the lhs", as finality is only a single possible case

---
