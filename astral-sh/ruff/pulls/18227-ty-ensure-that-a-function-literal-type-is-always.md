```yaml
number: 18227
title: "[ty] Ensure that a function-literal type is always equivalent to itself"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/property-test-failure
created_at: 2025-05-20T17:41:47Z
updated_at: 2025-05-20T18:11:06Z
url: https://github.com/astral-sh/ruff/pull/18227
synced_at: 2026-01-10T18:51:02Z
```

# [ty] Ensure that a function-literal type is always equivalent to itself

---

_Pull request opened by @AlexWaygood on 2025-05-20 17:41_

## Summary

This is a short-term fix to some of our type-equivalence logic for function-literal and bound-method types. The issue was that two `Callable` types will not be considered equivalent if any parameter in either type is unannotated (and it is obviously very common in methods for the `self` parameter to be unannotated!). Following https://github.com/astral-sh/ruff/commit/97058e80930969d63bc2cdc6fcff8f84a2d7de4e, equivalence of two `FunctionLiteral` types defers to equivalence of the `CallableType` supertypes of the two function-literal types.

Long-term, we should clarify this situation by splitting `FunctionLiteral` into two separate variants, as described by @dcreager in https://github.com/astral-sh/ty/issues/459#issuecomment-2895084351 and https://github.com/astral-sh/ty/issues/462

Closes https://github.com/astral-sh/ty/issues/459

## Test Plan

- `cargo test -p ty_python_semantic`
- `QUICKCHECK_TESTS=100000 cargo test --release -p ty_python_semantic -- --ignored types::property_tests::stable`


---

_Review requested from @carljm by @AlexWaygood on 2025-05-20 17:41_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-20 17:41_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-20 17:41_

---

_Label `ty` added by @AlexWaygood on 2025-05-20 17:41_

---

_@AlexWaygood reviewed on 2025-05-20 17:42_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:1 on 2025-05-20 17:42_

The changes in this file are not actually necessary to fix the bug, just a driveby fix for something that I noticed while fixing the bug

---

_Comment by @github-actions[bot] on 2025-05-20 17:44_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@dcreager reviewed on 2025-05-20 17:59_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:7169 on 2025-05-20 17:59_

Discussed in person — I think both `is_subtype_of` and `is_equivalent_to` should follow the pattern above, using the normalized check as a fast path, but falling back on the body scope + callable check that was there previously.

(I think a lot of this will hopefully be cleaned up as part of https://github.com/astral-sh/ty/issues/462)

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-20 18:01_

---

_@dcreager approved on 2025-05-20 18:02_

---

_Comment by @AlexWaygood on 2025-05-20 18:10_

1% slowdown on the cold benchmark: https://codspeed.io/astral-sh/ruff/branches/alex%2Fproperty-test-failure.

I think that's acceptable here. It might well be addressed when we do https://github.com/astral-sh/ty/issues/462.

---

_Merged by @AlexWaygood on 2025-05-20 18:11_

---

_Closed by @AlexWaygood on 2025-05-20 18:11_

---

_Branch deleted on 2025-05-20 18:11_

---
