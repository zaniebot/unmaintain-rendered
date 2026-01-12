```yaml
number: 15261
title: "[red-knot] Improve `Type::is_disjoint_from()` for `KnownInstanceType`s"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/known-instance-disjointness
created_at: 2025-01-04T18:36:37Z
updated_at: 2025-01-05T22:51:41Z
url: https://github.com/astral-sh/ruff/pull/15261
synced_at: 2026-01-12T15:55:50Z
```

# [red-knot] Improve `Type::is_disjoint_from()` for `KnownInstanceType`s

---

_@AlexWaygood_

## Summary

Two things are fixed here:
- If we have a known instance (e.g. `KnownInstance::TypingLiteral`) that was an instance of `typing._SpecialForm`, we currently don't recognise that this type is disjoint from a class `Foo` that is a subclass of `typing._SpecialForm`. This is a _bit_ theoretical, as `KnownInstance`s in our model are generally instances of classes that cannot be subclassed (at least, not easily). However, fixing this bug revealed (thanks to the property tests) that...
- We don't currently consider `Type::Tuple([])` to be disjoint from `Type::KnownInstance(KnownInstanceType::TypingLiteral)`.

## Test Plan

- Added a new unit test that ensures that `Type::Tuple([])` is now considered disjoint from `Type::KnownInstance(KnownInstanceType::TypingLiteral)`.
- Ran `QUICKCHECK_TESTS=200000 cargo test -p red_knot_python_semantic -- --ignored types::property_tests::stable` to ensure that there are no new failures in the property tests



---

_Label `red-knot` added by @AlexWaygood on 2025-01-04 18:36_

---

_Review requested from @carljm by @AlexWaygood on 2025-01-04 18:36_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-04 18:36_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-01-04 18:36_

---

_Comment by @github-actions[bot] on 2025-01-04 18:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2025-01-04 18:46_

Argh, no. This does create new property test failures. I must have had a fluky success locally.

---

_Converted to draft by @AlexWaygood on 2025-01-04 18:46_

---

_Marked ready for review by @AlexWaygood on 2025-01-04 18:59_

---

_Comment by @AlexWaygood on 2025-01-04 19:00_

Okay, I fixed the new bug surfaced by the property tests. This time it was that we weren't recognising `Type::KnownInstance(KnownInstanceType::TypingLiteral)` as being disjoint from `Type::SubclassOf(<builtins.object>)`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1216 on 2025-01-05 22:35_

Would it be less repetitive to delegate this to "the known instance type is disjoint from `KnownClass::Tuple.to_instance()`, which would end up in the prior case?

---

_@carljm approved on 2025-01-05 22:36_

Nice!

---

_@AlexWaygood reviewed on 2025-01-05 22:45_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1216 on 2025-01-05 22:45_

Ahh, great catch! I've pushed that change. (I checked that it doesn't cause any new property test failures, as well.)

---

_Merged by @AlexWaygood on 2025-01-05 22:49_

---

_Closed by @AlexWaygood on 2025-01-05 22:49_

---

_Branch deleted on 2025-01-05 22:49_

---
