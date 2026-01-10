```yaml
number: 13940
title: "[red-knot] - Flow-control for boolean operations"
type: pull_request
state: merged
author: TomerBin
labels:
  - ty
assignees: []
merged: true
base: main
head: tomer/bool-op-flow-control
created_at: 2024-10-26T19:28:20Z
updated_at: 2024-11-01T13:31:59Z
url: https://github.com/astral-sh/ruff/pull/13940
synced_at: 2026-01-10T20:59:37Z
```

# [red-knot] - Flow-control for boolean operations

---

_Pull request opened by @TomerBin on 2024-10-26 19:28_

## Summary

As python uses short-circuiting boolean operations in runtime, we should mimic that logic in redknot as well.
For example, we should detect that in the following code `x` might be undefined inside the block:

```py
if flag or (x := 1):
    print(x) 
```

## Test Plan

Added mdtest suit for boolean expressions. 


---

_Renamed from "Tomer/bool op flow control" to "[red-knot] - Flow-control for boolean operations" by @TomerBin on 2024-10-26 19:30_

---

_@TomerBin reviewed on 2024-10-26 19:33_

---

_Review comment by @TomerBin on `crates/red_knot_test/src/parser.rs`:425 on 2024-10-26 19:33_

I spent some time debugging why my test wasn't running 😅 

---

_Marked ready for review by @TomerBin on 2024-10-26 19:36_

---

_Review requested from @carljm by @TomerBin on 2024-10-26 19:36_

---

_Review requested from @MichaReiser by @TomerBin on 2024-10-26 19:36_

---

_Review requested from @AlexWaygood by @TomerBin on 2024-10-26 19:36_

---

_Comment by @github-actions[bot] on 2024-10-26 19:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/boolean/short_circuit.md`:13 on 2024-10-26 20:52_

nit: it's better to assert on the error code here rather than the error message, so that if we change the message in the future the test still passes (same comment for a few other places in this PR). It _would_ be good to assert on the error message if this was a place where it was ambiguous which variable the error was being emitted about, but here it's not :-)

```suggestion
    # error: [possibly-unresolved-reference]
```

---

_@AlexWaygood reviewed on 2024-10-26 20:58_

Nice!

---

_@carljm approved on 2024-10-27 03:29_

Looks great, thank you!! All I had on review were minor nits, so I just pushed those changes (including Alex's comment) and will go ahead and land.

---

_Merged by @carljm on 2024-10-27 03:33_

---

_Closed by @carljm on 2024-10-27 03:33_

---

_Label `red-knot` added by @dhruvmanila on 2024-11-01 13:31_

---
