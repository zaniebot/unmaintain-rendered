```yaml
number: 17464
title: "[red-knot] add fixpoint iteration for Type::member_lookup_with_policy"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/membercycle
created_at: 2025-04-18T16:27:12Z
updated_at: 2025-04-18T17:20:04Z
url: https://github.com/astral-sh/ruff/pull/17464
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] add fixpoint iteration for Type::member_lookup_with_policy

---

_@carljm_

## Summary

Member lookup can be cyclic, with type inference of implicit members. A sample case is shown in the added mdtest.

There's no clear way to handle such cases other than to fixpoint-iterate the cycle.

Fixes #17457.

## Test Plan

Added test.


---

_Label `red-knot` added by @carljm on 2025-04-18 16:27_

---

_Review requested from @AlexWaygood by @carljm on 2025-04-18 16:27_

---

_Review requested from @sharkdp by @carljm on 2025-04-18 16:27_

---

_Review requested from @dcreager by @carljm on 2025-04-18 16:27_

---

_@carljm reviewed on 2025-04-18 16:28_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:150 on 2025-04-18 16:28_

This is a drive-by change, just moving the `impl` for `AttributeKind` next to the struct definition.

---

_Comment by @github-actions[bot] on 2025-04-18 16:29_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@AlexWaygood reviewed on 2025-04-18 16:50_

Could we maybe add a few more test cases? I'd be interested in what happens for some variations of the test you've added, e.g.

```py
class C:
    def __init__(self):
        self.x = 1

    def copy(self, other: "C"):
        self.x: int = other.x

reveal_type(C().x)
```

and


```py
class C:
    def __init__(self):
        self.x: int = 1

    def copy(self, other: "C"):
        self.x = other.x

reveal_type(C().x)
```

and


```py
class C:
    def copy(self, other: "C"):
        self.x = other.x

reveal_type(C().x)
```

and

```py
class C:
    def copy(self, other: "C"):
        self.x: int = other.x

reveal_type(C().x)
```

---

_Comment by @carljm on 2025-04-18 16:57_

Sure. Three out of those four (numbers 1, 2, and 4) are pretty straightforward: if there's a declaration, we don't attempt to infer anything from the RHS, even of other assignments to the same name. So all of those work fine already on `main`, don't hit any cycle, and reveal `int` as you'd expect. I can still add them.

Number three is interesting, definitely worth adding. It has the result I'd expect from fixpoint iteration: we converge on `Unknown`.

---

_@AlexWaygood approved on 2025-04-18 17:07_

Nice. If the new panic MRE in https://github.com/astral-sh/ruff/issues/17457#issuecomment-2815848955 is the same bug, it might be worth adding that as a test case as well. But if it's a separate issue, it seems fine to defer that for now.

---

_Comment by @carljm on 2025-04-18 17:19_

That one's a separate issue and will have a totally separate fix. It's a case where we are already (correctly) fixpoint iterating, but iteration does not converge due to an issue in union simplification. We don't simplify `(Unknown & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy)`, so fixpoint iteration just keeps growing that union type with more identical elements, unboundedly (until we reach "too many iterations").

---

_Merged by @carljm on 2025-04-18 17:20_

---

_Closed by @carljm on 2025-04-18 17:20_

---

_Branch deleted on 2025-04-18 17:20_

---
