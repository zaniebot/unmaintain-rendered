```yaml
number: 19781
title: "[ty] Improve subscript narrowing for \"safe mutable classes\""
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/lazy-to-instance
created_at: 2025-08-06T10:56:25Z
updated_at: 2025-08-06T11:26:27Z
url: https://github.com/astral-sh/ruff/pull/19781
synced_at: 2026-01-12T15:56:46Z
```

# [ty] Improve subscript narrowing for "safe mutable classes"

---

_@AlexWaygood_

## Summary

This PR improves the `is_safe_mutable_class` function in `infer.rs` in several ways:
- It uses `KnownClass::to_instance()` for all "safe mutable classes". Previously, we were using `SpecialFormType::instance_fallback()` for some variants -- I'm not totally sure why. Switching to `KnownClass::to_instance()` for all "safe mutable classes" fixes a number of TODOs in the `assignment.md` mdtest suite
- Rather than eagerly calling `.to_instance(db)` on all "safe mutable classes" every time `is_safe_mutable_class` is called, we now only call it lazily on each element, allowing us to short-circuit more effectively.
- I removed the entry entirely for `TypedDict` from the list of "safe mutable classes", as it's not correct. `SpecialFormType::TypedDict.instance_fallback(db)` just returns an instance type representing "any instance of `typing._SpecialForm`", which I don't think was the intent of this code. No tests fail as a result of removing this entry, as we already check separately whether an object is an inhabitant of a `TypedDict` type (and consider that object safe-mutable if so!).

## Test Plan

mdtests updated


---

_Label `ty` added by @AlexWaygood on 2025-08-06 10:56_

---

_Comment by @github-actions[bot] on 2025-08-06 10:58_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-06 11:00_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-08-06 11:02_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-06 11:02_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-06 11:02_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-06 11:02_

---

_@sharkdp approved on 2025-08-06 11:21_

Thank you!

> I removed the entry entirely for TypedDict from the list of "safe mutable classes", as it's not correct. SpecialFormType::TypedDict.instance_fallback(db) just returns an instance type representing "any instance of typing._SpecialForm", which I don't think was the intent of this code. No tests fail as a result of removing this entry, as we already check separately whether an object is an inhabitant of a TypedDict type (and consider that object safe-mutable if so!).

I saw that entry as well, was confused, and made a mental note to check it again. Thanks for fixing it.

---

_Merged by @AlexWaygood on 2025-08-06 11:26_

---

_Closed by @AlexWaygood on 2025-08-06 11:26_

---

_Branch deleted on 2025-08-06 11:26_

---
