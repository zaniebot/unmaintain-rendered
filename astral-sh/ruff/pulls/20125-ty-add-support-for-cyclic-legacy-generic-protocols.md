```yaml
number: 20125
title: "[ty] add support for cyclic legacy generic protocols"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/cycliclegacygenericprotocols
created_at: 2025-08-28T03:04:14Z
updated_at: 2025-08-28T16:58:02Z
url: https://github.com/astral-sh/ruff/pull/20125
synced_at: 2026-01-10T17:46:21Z
```

# [ty] add support for cyclic legacy generic protocols

---

_Pull request opened by @carljm on 2025-08-28 03:04_

## Summary

Just add the necessary Salsa cycle handling.

## Test Plan

Added mdtest.


---

_Label `ty` added by @carljm on 2025-08-28 03:04_

---

_Comment by @github-actions[bot] on 2025-08-28 03:06_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-28 03:08_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @carljm on 2025-08-28 03:39_

---

_Review requested from @AlexWaygood by @carljm on 2025-08-28 03:39_

---

_Review requested from @sharkdp by @carljm on 2025-08-28 03:39_

---

_Review requested from @dcreager by @carljm on 2025-08-28 03:39_

---

_@sharkdp approved on 2025-08-28 07:01_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:2455 on 2025-08-28 09:28_

Rather than creating a new `## Cycles` section, could you put this new test case in the existing `## Recursive protocols` section here?

https://github.com/astral-sh/ruff/blob/18eaa659c1ea9a03bee798e161d2f2db454e154f/crates/ty_python_semantic/resources/mdtest/protocols.md?plain=1#L2045

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6594 on 2025-08-28 09:29_

is `Never` the best starting point here, or would we finish cycle recovery faster if we returned `self`?

---

_@AlexWaygood approved on 2025-08-28 09:29_

Thank you!

---

_@carljm reviewed on 2025-08-28 16:53_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6594 on 2025-08-28 16:53_

Hmm, it's a good question. My instinct is that returning `self` would only help us resolve faster if `self` is actually the right answer (that is, the specialization shouldn't change anything), and I think that wouldn't be common (not even sure if we would hit the cycle in the first place if that were the case.) I don't think it's going to make much difference either way, I'd rather not devote more time right now to exploring it, and I'm more comfortable with `Never` as a starting point.

---

_Merged by @carljm on 2025-08-28 16:58_

---

_Closed by @carljm on 2025-08-28 16:58_

---

_Branch deleted on 2025-08-28 16:58_

---
