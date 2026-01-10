```yaml
number: 18669
title: "[ty] Delay computation of 'unbound' visibility for implicit instance attributes"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-627
created_at: 2025-06-13T19:32:32Z
updated_at: 2025-06-13T19:50:58Z
url: https://github.com/astral-sh/ruff/pull/18669
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Delay computation of 'unbound' visibility for implicit instance attributes

---

_Pull request opened by @sharkdp on 2025-06-13 19:32_

## Summary

Consider the following example, which leads to a excessively large runtime on `main`. The reason for this is the following. When inferring types for `self.a`, we look up the `a` attribute on `C`. While looking for implicit instance attributes, we go through every method and check for `self.a = …` assignments. There are no such assignments here, but we always have an implicit `self.a = <unbound>` binding at the beginning over every method. This binding accumulates a complex visibility constraint in `C.f`, due to the `isinstance` checks. While evaluating that constraint, we need to infer the type of `self.b`. There's no binding for `self.b` either, but there's also an implicit `self.b = <unbound>` binding with the same complex visibility constraint (involving `self.b` recursively). This leads to a combinatorial explosion:

```py
class C:
    def f(self: "C"):
        if isinstance(self.a, str):
            return

        if isinstance(self.b, str):
            return
        if isinstance(self.b, str):
            return
        if isinstance(self.b, str):
            return
        # repeat 20 times
```
(note that the `self` parameter here is annotated explicitly because we currently still infer `Unknown` for `self` otherwise)

The fix proposed here is rather simple: when there are no `self.name = …` attribute assignments in a given method, we skip evaluating the visibility constraint of the implicit `self.name = <unbound>` binding. This should also generally help with performance, because that's a very common case.

This is *not* a fix for cases where there *are* actual bindings in the method. When we add `self.a = 1; self.b = 1` to that example above, we still see that combinatorial explosion of runtime. I still think it's worth to make this optimization, as it fixes the problems with `pandas` and `sqlalchemy` reported by users. I will open a ticket to track that separately.

closes https://github.com/astral-sh/ty/issues/627
closes https://github.com/astral-sh/ty/issues/641

## Test Plan

* Made sure that `ty` finishes quickly on the MREs in https://github.com/astral-sh/ty/issues/627
* Made sure that `ty` finishes quickly on `pandas`
* Made sure that `ty` finishes quickly on `sqlalchemy`

---

_Review requested from @carljm by @sharkdp on 2025-06-13 19:32_

---

_Label `ty` added by @sharkdp on 2025-06-13 19:32_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-06-13 19:32_

---

_Review requested from @dcreager by @sharkdp on 2025-06-13 19:32_

---

_Comment by @sharkdp on 2025-06-13 19:37_

We should probably also add a regression test/benchmark. I can look into that next week, in a follow-up, in case we want to merge this sooner.

---

_Comment by @github-actions[bot] on 2025-06-13 19:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-06-13 19:50_

Looks good, thank you!

---

_Merged by @carljm on 2025-06-13 19:50_

---

_Closed by @carljm on 2025-06-13 19:50_

---

_Branch deleted on 2025-06-13 19:50_

---
