```yaml
number: 19884
title: "[ty] simplify return type of place_from_declarations"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/place-from-decls
created_at: 2025-08-12T19:05:37Z
updated_at: 2025-08-13T14:26:10Z
url: https://github.com/astral-sh/ruff/pull/19884
synced_at: 2026-01-10T17:52:17Z
```

# [ty] simplify return type of place_from_declarations

---

_Pull request opened by @carljm on 2025-08-12 19:05_

## Summary

A [passing comment](https://github.com/astral-sh/ruff/pull/19711#issuecomment-3169312014) led me to explore why we didn't report a class attribute as possibly unbound if it was a method and defined in two different conditional branches.

I found that the reason was because of our handling of "conflicting declarations" in `place_from_declarations`. It returned a `Result` which would be `Err` in case of conflicting declarations.

But we only actually care about conflicting declarations when we are actually doing type inference on that scope and might emit a diagnostic about it. And in all cases (including that one), we want to otherwise proceed with the union of the declared types, as if there was no conflict.

In several cases we were failing to handle the union of declared types in the same way as a normal declared type if there was a declared-types conflict. The `Result` return type made this mistake really easy to make, as we'd match on e.g. `Ok(Place::Type(...))` and do one thing, then match on `Err(...)` and do another, even though really both of those cases should be handled the same.

This PR refactors `place_from_declarations` to instead return a struct which always represents the declared type we should use in the same way, as well as carrying the conflicting declared types, if any. This struct has a method to allow us to explicitly ignore the declared-types conflict (which is what we want in most cases), as well as a method to get the declared type and the conflict information, in the case where we want to emit a diagnostic on the conflict.

## Test Plan

Existing CI; added a test showing that we now understand a multiply-conditionally-defined method as possibly-unbound.

This does trigger issues on a couple new fuzzer seeds, but the issues are just new instances of an already-known (and rarely occurring) problem which I already plan to address in a future PR, so I think it's OK to land as-is.

I happened to build this initially on top of https://github.com/astral-sh/ruff/pull/19711, which adds invalid-await diagnostics, so I also updated some invalid-syntax tests to not await on an invalid type, since the purpose of those tests is to check the syntactic location of the `await`, not the validity of the awaited type.


---

_Label `ty` added by @carljm on 2025-08-12 19:05_

---

_Comment by @github-actions[bot] on 2025-08-12 19:07_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-13 14:15:20.459792009 +0000
+++ new-output.txt	2025-08-13 14:15:20.528792019 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(218b3)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(2367a)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-12 19:10_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-08-13 01:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @carljm on 2025-08-13 02:00_

---

_Review requested from @AlexWaygood by @carljm on 2025-08-13 02:00_

---

_Review requested from @sharkdp by @carljm on 2025-08-13 02:00_

---

_Review requested from @dcreager by @carljm on 2025-08-13 02:00_

---

_Comment by @MichaReiser on 2025-08-13 06:32_

Thank you for looking into this.

> But we only actually care about conflicting declarations when we are actually doing type inference on that scope and might emit a diagnostic about it. And in all cases (including that one), we want to otherwise proceed with the union of the declared types, as if there was no conflict.

To me, this doesn't sound much different to the `Type::try_*` methods that all return a `Result`. It seems that the main issue isn't that the return type is a `Result`, but that

1. We call `.ok()` on the result in some places when we shouldn't
2. We don't track the `PlaceAndQualifiers` for declarations with conflicts.

I don't feel strongly but I would prefer, if possible without much added complexity, to follow the same pattern that we use in our `Type::try_` methods instead of establishing another pattern. Which means, `place_from_declarations` would still return a `Result` but the type wrapped in the `Err` variant has a method to get the `PlaceAndQualifiers` for situations where we don't care about conflicting declarations. 

---

_Comment by @carljm on 2025-08-13 13:54_

> this doesn't sound much different to the `Type::try_*` methods

It seems quite different to me, because `place_from_declarations` never "fails" -- it always succeeds, and always succeeds using exactly the same logic to determine its return type. It just in some cases also provides extra information that some callers may want to use to emit a diagnostic for the user. The key difference is that we _always_ (at literally every possibly callsite) want to handle the returned value in _exactly_ the same way, we _never_ want to distinguish the handling between Ok and Err cases (except that in one callsite we _additionally_ want to emit a diagnostic if conflicting-types were returned -- but we still handle the main return value identically.)

I think a better parallel is our `Place` type (the Bound and PossiblyUnbound cases), where we have a `.ignore_possibly_unbound()` method.

So I think this is following existing patterns in the codebase, just different ones (which I think are more similar to this case.)

> `place_from_declarations` would still return a `Result` but the type wrapped in the `Err` variant has a method to get the `PlaceAndQualifiers` for situations where we don't care about conflicting declarations.

I don't think this would address the problem I aimed to fix in this PR.  It isn't possible to add an `.ignore_conflicting_declarations()` method directly to `Result`, so it would remain the case that we would need separate paths in every callsite to handle the `Ok` and `Err` versions, introducing the risk that we fail to handle them the same. A match arm for `Ok(...)` would not match the `Err` variant, leading to bugs.

---

_Comment by @MichaReiser on 2025-08-13 14:00_

> It seems quite different to me, because place_from_declarations never "fails" -- it always succeeds, and always succeeds using exactly the same logic to determine its return type.

Thanks, I think this is the key distinction that wasn't clear to me

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/place.rs`:505 on 2025-08-13 14:02_

Nit: The `with` naming here suggests that this creates a new `PlaceFromDeclarationResult`. Should this be `into_conflicting_declarations` or similar?

---

_@MichaReiser approved on 2025-08-13 14:03_

---

_@carljm reviewed on 2025-08-13 14:07_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/place.rs`:505 on 2025-08-13 14:07_

Yeah, I wasn't sure of the best naming for this method, probably the most clear name would be `into_place_and_qualifiers_and_conflicting_declarations` but that's quite a mouthful :) On the other hand, there's only one callsite, so maybe a mouthful is fine.

---

_Merged by @carljm on 2025-08-13 14:17_

---

_Closed by @carljm on 2025-08-13 14:17_

---

_Branch deleted on 2025-08-13 14:17_

---
