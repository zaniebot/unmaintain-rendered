```yaml
number: 16076
title: "[red-knot] Reduce usage of `From<Type>` implementations when working with `Symbol`s"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/simplify-symbol-usage
created_at: 2025-02-10T13:47:25Z
updated_at: 2025-02-11T11:09:38Z
url: https://github.com/astral-sh/ruff/pull/16076
synced_at: 2026-01-12T15:55:53Z
```

# [red-knot] Reduce usage of `From<Type>` implementations when working with `Symbol`s

---

_@AlexWaygood_

## Summary

This PR removes the `From<Type>` implementations for `Symbol` and `SymbolAndQualifiers`, replacing them with smart constructors. This has a few advantages:
- It's easier to see when reading the code what (Rust) type the variable is being turned into, compared to a `.into()` call
- It's more explicit that we're creating symbols with `Boundness::Bound`
- In some cases, it's more concise

## Test Plan

<!-- How was it tested? -->


---

_Label `red-knot` added by @AlexWaygood on 2025-02-10 13:47_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-10 13:47_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-10 13:47_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-10 13:47_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:406 on 2025-02-10 19:05_

Minor naming nit: I think usually a "smart constructor" is understood to apply some additional consistency checks or simplifications. I wouldn't call this a smart constructor, just... a constructor.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2592 on 2025-02-10 19:05_

Same comment as above.

---

_@carljm approved on 2025-02-10 19:09_

I agree this is a bit clearer for the reader.

---

_Merged by @AlexWaygood on 2025-02-11 11:09_

---

_Closed by @AlexWaygood on 2025-02-11 11:09_

---

_Branch deleted on 2025-02-11 11:09_

---
