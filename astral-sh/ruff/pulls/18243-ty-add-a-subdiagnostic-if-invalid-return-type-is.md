```yaml
number: 18243
title: "[ty] Add a subdiagnostic if `invalid-return-type` is emitted on a method with an empty body on a non-protocol subclass of a protocol class"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/invalid-return-proto-note
created_at: 2025-05-21T16:17:49Z
updated_at: 2025-05-21T17:38:08Z
url: https://github.com/astral-sh/ruff/pull/18243
synced_at: 2026-01-12T15:56:15Z
```

# [ty] Add a subdiagnostic if `invalid-return-type` is emitted on a method with an empty body on a non-protocol subclass of a protocol class

---

_@AlexWaygood_

## Summary

Fixes https://github.com/astral-sh/ty/issues/473.

If `invalid-return-type` is emitted on a method with an empty body on a non-protocol subclass of a protocol class, the user probably meant for the non-protocol class to actually be a protocol class too. Add a nice subdiagnostic explaining why we do not infer the concrete subclass as being a protocol class.

## Test Plan

snapshots


---

_Review requested from @carljm by @AlexWaygood on 2025-05-21 16:17_

---

_Label `ty` added by @AlexWaygood on 2025-05-21 16:17_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-21 16:17_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-21 16:17_

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-21 16:17_

---

_Comment by @github-actions[bot] on 2025-05-21 16:20_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:1705 on 2025-05-21 16:54_

We already have a function `enclosing_class_symbol` (which should probably be renamed because it returns a Type,  not a Symbol) -- seems like there's likely redundancy between this method and that one?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:1696 on 2025-05-21 16:56_

Given that there were two different uses of this method, and both now become significantly more verbose, would it really be a problem to keep this method as short-hand, even though it's body would now be a one-liner?

---

_@carljm approved on 2025-05-21 16:56_

Looks good, thank you!

---

_@AlexWaygood reviewed on 2025-05-21 16:59_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:1696 on 2025-05-21 16:59_

In one of the two use cases (the one where we now want to retain the enclosing class context even if the class is not a protocol class), I think it would be a mistake to use this method, because you'd end up doing a lot of work twice in quick succession. So I feel like it's sort-of a footgun to keep it?

---

_@AlexWaygood reviewed on 2025-05-21 17:04_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:1705 on 2025-05-21 17:04_

Ah, that existing function continues iterating up through scopes until it finds a class scope, whereas this new function should only return `Some(class)` if `class` is either the immediate parent scope _or_ the immediate parent scope is a type-parameters scope and `class` is the grandparent scope. So they're doing subtly different things. I'll rename the other one and add doc-comments to both to explain the differences.

---

_Merged by @AlexWaygood on 2025-05-21 17:38_

---

_Closed by @AlexWaygood on 2025-05-21 17:38_

---

_Branch deleted on 2025-05-21 17:38_

---
