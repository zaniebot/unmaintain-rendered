```yaml
number: 20114
title: "[ty] Preserve qualifiers when accessing attributes on unions/intersections"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/attribute-access-on-intersections-again
created_at: 2025-08-27T14:57:35Z
updated_at: 2025-08-27T18:01:47Z
url: https://github.com/astral-sh/ruff/pull/20114
synced_at: 2026-01-12T15:56:54Z
```

# [ty] Preserve qualifiers when accessing attributes on unions/intersections

---

_@sharkdp_

## Summary

Properly preserve type qualifiers when accessing attributes on unions and intersections. This is a prerequisite for https://github.com/astral-sh/ruff/pull/19579.

Also fix a completely wrong implementation of `map_with_boundness_and_qualifiers`. It now closely follows `map_with_boundness` (just above).

## Test Plan

I thought about it, but didn't find any easy way to test this. This only affected `Type::member`. Things like validation of attribute writes (where type qualifiers like `ClassVar` and `Final` are important) were already handling things correctly.


---

_Label `ty` added by @sharkdp on 2025-08-27 14:57_

---

_Renamed from "[ty] Fix attribute access on intersection types" to "[ty] Preserve qualifiers when accessing attributes on unions/intersections" by @sharkdp on 2025-08-27 14:58_

---

_Comment by @github-actions[bot] on 2025-08-27 14:59_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-27 15:01_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@sharkdp reviewed on 2025-08-27 15:19_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/annotations/callable.md`:401 on 2025-08-27 15:19_

Slightly surprised that there was no TODO here. This looks fine?

---

_@sharkdp reviewed on 2025-08-27 15:20_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:9742 on 2025-08-27 15:20_

The implementation here now follows `map_with_boundness` just above. It was wrong before (see test change in `callable.md`).

---

_Marked ready for review by @sharkdp on 2025-08-27 15:20_

---

_Review requested from @carljm by @sharkdp on 2025-08-27 15:20_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-08-27 15:20_

---

_Review requested from @dcreager by @sharkdp on 2025-08-27 15:20_

---

_Converted to draft by @sharkdp on 2025-08-27 15:24_

---

_Marked ready for review by @sharkdp on 2025-08-27 15:28_

---

_@AlexWaygood reviewed on 2025-08-27 15:57_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/callable.md`:401 on 2025-08-27 15:57_

yes, this looks fine!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9755 on 2025-08-27 16:01_

```suggestion
                    any_definitely_bound |= member_boundness == Boundness::Bound;
```

---

_@AlexWaygood approved on 2025-08-27 16:01_

---

_@carljm approved on 2025-08-27 16:02_

LGTM, thank you!

---

_@sharkdp reviewed on 2025-08-27 18:01_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:9755 on 2025-08-27 18:01_

I think I prefer what we have in terms of readability. Not sure if it compiles to worse code (probably not?), but this is not a hot code path anyway.

---

_Merged by @sharkdp on 2025-08-27 18:01_

---

_Closed by @sharkdp on 2025-08-27 18:01_

---

_Branch deleted on 2025-08-27 18:01_

---
