```yaml
number: 15105
title: "[red-knot] Treat classes as instances of their respective metaclasses in boolean tests"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-class-truthiness
created_at: 2024-12-23T00:18:32Z
updated_at: 2024-12-23T04:44:24Z
url: https://github.com/astral-sh/ruff/pull/15105
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Treat classes as instances of their respective metaclasses in boolean tests

---

_Pull request opened by @InSyncWithFoo on 2024-12-23 00:18_

## Summary

Follow-up to #15089.

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2024-12-23 00:18_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2024-12-23 00:18_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2024-12-23 00:18_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2024-12-23 00:18_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1682 on 2024-12-23 00:24_

I think all of the tests in this PR exercise only the below case (`SubclassOf`), because that's what a `type[...]` annotation spells; I don't think any of them exercise this `ClassLiteral` case. To do that, we'll want to add some tests directly with the class name itself, rather than `type[..]` annotation.

---

_Comment by @github-actions[bot] on 2024-12-23 00:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:263 on 2024-12-23 00:27_

Something like this (plus adding `flag: bool` to the parameter list for above) should be sufficient to test the `ClassLiteral` case:
```suggestion
        reveal_type(d)  # revealed: type[DeferredClass] & ~AlwaysFalsy
        
        tf = TruthyClass if flag else FalsyClass
        reveal_type(tf)  # Literal[TruthyClass, FalsyClass]
        if tf:
            reveal_type(tf)  # Literal[TruthyClass]
        else:
            reveal_type(tf)  # Literal[FalsyClass]
```

---

_@carljm reviewed on 2024-12-23 00:27_

Looks great, thank you! Just one area where I think we should add some test coverage.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:249 on 2024-12-23 00:40_

```suggestion
    af: type[AmbiguousClass] | type[FalsyClass],
    flag: bool,
```

---

_@carljm reviewed on 2024-12-23 00:40_

---

_Merged by @carljm on 2024-12-23 01:30_

---

_Closed by @carljm on 2024-12-23 01:30_

---

_Branch deleted on 2024-12-23 01:33_

---

_Label `red-knot` added by @dhruvmanila on 2024-12-23 04:44_

---
