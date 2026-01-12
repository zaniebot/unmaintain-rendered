```yaml
number: 20775
title: "[ty] Fix broken property tests for disjointness of intersections"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/fix-property-tests
created_at: 2025-10-08T20:44:58Z
updated_at: 2025-10-08T21:28:58Z
url: https://github.com/astral-sh/ruff/pull/20775
synced_at: 2026-01-12T15:57:09Z
```

# [ty] Fix broken property tests for disjointness of intersections

---

_@AlexWaygood_

## Summary

Two stable property tests are currently failing on `main`, following https://github.com/astral-sh/ruff/commit/f054b8a55e759f3afa9d9cf0acf819185228a739 (of course, I only thought to run the property tests again around 30 minutes _after_ landing that PR...). The issue is quite subtle, and took me an annoying amount of time to pin down: we're matching over `(self, other)` in `Type::is_disjoint_from_impl`, but `other` here is shadowed by the binding in the `match` branch, which means that the wrong key is inserted into the cache of the `IsDisjointFrom` cycle detector:

https://github.com/astral-sh/ruff/blob/f054b8a55e759f3afa9d9cf0acf819185228a739/crates/ty_python_semantic/src/types.rs#L2408-L2435

This PR fixes that issue, and also adds a few `Debug` implementations to our cycle detectors, so that issues like this are easier to debug in the future.

I'm adding the `internal` label, as this fixes a bug that hasn't yet appeared in any released version of ty, so it doesn't deserve its own changelog entry.

## Test Plan

`QUICKCHECK_TESTS=1000000 cargo test --release -p ty_python_semantic -- --ignored types::property_tests::stable` now once again passes on `main`

I considered adding new mdtests as well, but the examples that the property tests were throwing at me all seemed _quite_ obscure and somewhat unlikely to occur in the real world. I don't think it's worth it.

---

_Review requested from @carljm by @AlexWaygood on 2025-10-08 20:45_

---

_Label `internal` added by @AlexWaygood on 2025-10-08 20:45_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-08 20:45_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-08 20:45_

---

_Label `ty` added by @AlexWaygood on 2025-10-08 20:45_

---

_Comment by @github-actions[bot] on 2025-10-08 20:47_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-08 20:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- Found 52 diagnostics
+ Found 51 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-10-08 21:04_

An example failure on `main`:

<details>

```
running 1 test
test types::property_tests::stable::disjoint_from_is_symmetric ... FAILED

failures:

---- types::property_tests::stable::disjoint_from_is_symmetric stdout ----

Failing types were:
(((...) -> None) & type[Any] & ~((*, n0) -> Unknown)) | (((...) -> None) & Mock & ~((*, n0) -> Unknown)) | (((...) -> None) & <class 'Mock'> & ~((*, n0) -> Unknown))
tuple[AlwaysFalsy] | (bound method str.isascii() -> bool) | tuple[Literal[True], AlwaysFalsy]

Failing types were:
(((...) -> None) & type[Any]) | (((...) -> None) & Mock) | (((...) -> None) & <class 'Mock'>)
tuple[AlwaysFalsy] | (bound method str.isascii() -> bool) | tuple[Literal[True], AlwaysFalsy]

Failing types were:
(((...) -> None) & type[Any]) | (((...) -> None) & Mock)
tuple[AlwaysFalsy] | (bound method str.isascii() -> bool) | tuple[Literal[True], AlwaysFalsy]

Failing types were:
(((...) -> None) & type[Any]) | (((...) -> None) & Mock)
(bound method str.isascii() -> bool) | tuple[Literal[True], AlwaysFalsy]

Failing types were:
(((...) -> None) & type[Any]) | (((...) -> None) & Mock)
(bound method str.isascii() -> bool) | tuple[Literal[True], AlwaysFalsy]

thread 'types::property_tests::stable::disjoint_from_is_symmetric' panicked at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (Intersection { pos: [Callable { params: GradualForm, returns: Some(None) }, Union([SubclassOfAny, UnittestMockInstance])], neg: [] }, Union([BuiltinsBoundMethod { class: "str", method: "isascii" }, FixedLengthTuple([BooleanLiteral(true), AlwaysFalsy])]))
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    types::property_tests::stable::disjoint_from_is_symmetric
```

---

_@carljm approved on 2025-10-08 21:18_

Nice find!

---

_Merged by @AlexWaygood on 2025-10-08 21:28_

---

_Closed by @AlexWaygood on 2025-10-08 21:28_

---

_Branch deleted on 2025-10-08 21:28_

---
