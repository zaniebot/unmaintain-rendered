```yaml
number: 16608
title: "[red-knot] Add tests asserting that `KnownClass::to_instance()` doesn't unexpectedly fallback to `Type::Unknown` with full typeshed stubs"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/knownclass-tests
created_at: 2025-03-10T17:44:37Z
updated_at: 2025-03-11T16:14:37Z
url: https://github.com/astral-sh/ruff/pull/16608
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Add tests asserting that `KnownClass::to_instance()` doesn't unexpectedly fallback to `Type::Unknown` with full typeshed stubs

---

_Pull request opened by @AlexWaygood on 2025-03-10 17:44_

## Summary

One of the motivations in https://github.com/astral-sh/ruff/pull/16428 for panicking when the `test` or `debug_assertions` features are enabled and a lookup of a `KnownClass` fails is that we've had some latent bugs in our code where certain variants have been silently falling back to `Unknown` in every typeshed lookup without us realising. But that in itself isn't a great motivation for panicking in `KnownClass::to_instance()`, since we can fairly easily add some tests that assert that we don't unexpectedly fallback to `Unknown` for any `KnownClass` variant. This PR adds those tests. 

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `testing` added by @AlexWaygood on 2025-03-10 17:44_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-10 17:44_

---

_Review requested from @carljm by @AlexWaygood on 2025-03-10 17:44_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-03-10 17:44_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-03-10 17:44_

---

_Comment by @github-actions[bot] on 2025-03-10 17:50_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:1661 on 2025-03-10 19:09_

Doesn't really matter, but it seems like it would be simpler and more robust just to unconditionally set the version each time through the loop, to the version-added for that class. Does that add noticeable runtime?

---

_@carljm approved on 2025-03-10 19:10_

Thank you!

---

_@AlexWaygood reviewed on 2025-03-11 16:09_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class.rs`:1661 on 2025-03-11 16:09_

> Does that add noticeable runtime?

nah, not at all. Not sure what I was thinking there, that's obviously simpler! Thanks!

---

_Merged by @AlexWaygood on 2025-03-11 16:12_

---

_Closed by @AlexWaygood on 2025-03-11 16:12_

---

_Branch deleted on 2025-03-11 16:12_

---
