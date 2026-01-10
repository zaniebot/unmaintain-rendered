```yaml
number: 20720
title: "[ty] Print `display` of types when a property test fails"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/property-test-debug
created_at: 2025-10-06T13:34:27Z
updated_at: 2025-10-06T13:44:26Z
url: https://github.com/astral-sh/ruff/pull/20720
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Print `display` of types when a property test fails

---

_Pull request opened by @AlexWaygood on 2025-10-06 13:34_

## Summary

This PR attempts to make it easier to understand what a failing property test _means_, by showing you (something closer to) the type that ty internally simplified a type to, rather than just the schema that was used to generate a pair of types for a particular property test.

Demo:

```
~/dev/ruff (alex/property-test-debug)⚡ [101] % QUICKCHECK_TESTS=1000000 cargo test --release -p ty_python_semantic -- --ignored types::property_tests::flaky::double_negation_is_identity
    Finished `release` profile [optimized] target(s) in 0.09s
     Running unittests src/lib.rs (target/release/deps/ty_python_semantic-8dac72c754bdebcd)

running 1 test
test types::property_tests::flaky::double_negation_is_identity ... FAILED

failures:

---- types::property_tests::flaky::double_negation_is_identity stdout ----

Failing types were:
(~(bound method int.bit_length() -> int) & ~Mock) | (bound method int.bit_length() -> int) | ABCMeta | ((**n4: tuple[*tuple[type[str], ...], Unknown, bool]) -> Literal[SafeUUID.unknown])

Failing types were:
(~(bound method int.bit_length() -> int) & ~Mock) | (bound method int.bit_length() -> int) | ABCMeta

Failing types were:
(~(bound method int.bit_length() -> int) & ~Mock) | (bound method int.bit_length() -> int)

Failing types were:
(~(bound method int.bit_length() -> int) & ~Mock) | (bound method int.bit_length() -> int)

thread 'types::property_tests::flaky::double_negation_is_identity' panicked at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (Union([Intersection { pos: [], neg: [BuiltinsBoundMethod { class: "int", method: "bit_length" }, UnittestMockInstance] }, BuiltinsBoundMethod { class: "int", method: "bit_length" }]))
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    types::property_tests::flaky::double_negation_is_identity
```

## Test Plan

<!-- How was it tested? -->


---

_Review requested from @carljm by @AlexWaygood on 2025-10-06 13:34_

---

_Label `internal` added by @AlexWaygood on 2025-10-06 13:34_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-06 13:34_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-06 13:34_

---

_Label `ty` added by @AlexWaygood on 2025-10-06 13:34_

---

_Comment by @github-actions[bot] on 2025-10-06 13:36_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-06 13:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- Found 53 diagnostics
+ Found 52 diagnostics

```
</details>
No memory usage changes detected ✅


---

_@sharkdp approved on 2025-10-06 13:43_

Thank you!

---

_Merged by @AlexWaygood on 2025-10-06 13:44_

---

_Closed by @AlexWaygood on 2025-10-06 13:44_

---

_Branch deleted on 2025-10-06 13:44_

---
