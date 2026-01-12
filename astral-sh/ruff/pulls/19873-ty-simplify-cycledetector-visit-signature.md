```yaml
number: 19873
title: "[ty] simplify CycleDetector::visit signature"
type: pull_request
state: merged
author: carljm
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: cjm/visitor-simpler
created_at: 2025-08-11T23:38:40Z
updated_at: 2025-08-12T00:12:27Z
url: https://github.com/astral-sh/ruff/pull/19873
synced_at: 2026-01-12T15:56:49Z
```

# [ty] simplify CycleDetector::visit signature

---

_@carljm_

## Summary

After https://github.com/astral-sh/ruff/pull/19871, I realized that now that we are passing around shared references to `CycleDetector` visitors, we can now also simplify the `visit` callback signature; we don't need to smuggle a single visitor reference through it anymore. This is a pretty minor simplification, and it doesn't really make anything shorter since I typically used a very short name (`v`) for the smuggled reference, but I think it reduces cognitive overhead in reading these `visit` usages; the extra variable would likely be confusing otherwise for a reader.

## Test Plan

Existing CI.


---

_Review requested from @AlexWaygood by @carljm on 2025-08-11 23:38_

---

_Review requested from @sharkdp by @carljm on 2025-08-11 23:38_

---

_Review requested from @dcreager by @carljm on 2025-08-11 23:38_

---

_Label `internal` added by @carljm on 2025-08-11 23:38_

---

_Label `ty` added by @carljm on 2025-08-11 23:38_

---

_Comment by @github-actions[bot] on 2025-08-11 23:40_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-11 23:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@AlexWaygood approved on 2025-08-11 23:51_

---

_Merged by @carljm on 2025-08-12 00:12_

---

_Closed by @carljm on 2025-08-12 00:12_

---

_Branch deleted on 2025-08-12 00:12_

---
