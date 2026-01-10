```yaml
number: 305
title: Daily property test run failed on Sat May 10 2025
type: issue
state: closed
author: github-actions
labels:
  - bug
  - testing
assignees: []
created_at: 2025-05-10T12:12:04Z
updated_at: 2025-05-11T09:27:28Z
url: https://github.com/astral-sh/ty/issues/305
synced_at: 2026-01-10T02:34:09Z
```

# Daily property test run failed on Sat May 10 2025

---

_Issue opened by @github-actions on 2025-05-10 12:12_

Run listed here: https://github.com/astral-sh/ruff/actions/runs/14945187071

---

_Label `bug` added by @github-actions[bot] on 2025-05-10 12:12_

---

_Label `testing` added by @github-actions[bot] on 2025-05-10 12:12_

---

_Label `bug` added by @AlexWaygood on 2025-05-10 12:15_

---

_Label `testing` added by @AlexWaygood on 2025-05-10 12:15_

---

_Comment by @AlexWaygood on 2025-05-10 12:16_

```
failures:

---- types::property_tests::stable::all_fully_static_type_pairs_are_subtype_of_their_union stdout ----

thread 'types::property_tests::stable::all_fully_static_type_pairs_are_subtype_of_their_union' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (SubclassOfBuiltinClass("str"), KnownClassInstance(List))
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

---- types::property_tests::stable::equivalent_to_is_reflexive stdout ----

thread 'types::property_tests::stable::equivalent_to_is_reflexive' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (KnownClassInstance(List))

---- types::property_tests::stable::subtype_of_is_reflexive stdout ----

thread 'types::property_tests::stable::subtype_of_is_reflexive' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (Callable { params: List([Param { kind: Variadic, name: Some(Name("n0")), annotated_ty: Some(BuiltinsFunction("chr")), default_ty: None }, Param { kind: KeywordVariadic, name: Some(Name("n8")), annotated_ty: Some(Tuple([KnownClassInstance(TypeAliasType)])), default_ty: None }]), returns: Some(KnownClassInstance(List)) })

---- types::property_tests::stable::two_gradual_equivalent_fully_static_types_are_also_equivalent stdout ----

thread 'types::property_tests::stable::two_gradual_equivalent_fully_static_types_are_also_equivalent' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (KnownClassInstance(Tuple), KnownClassInstance(Tuple))


failures:
    types::property_tests::stable::all_fully_static_type_pairs_are_subtype_of_their_union
    types::property_tests::stable::equivalent_to_is_reflexive
    types::property_tests::stable::subtype_of_is_reflexive
    types::property_tests::stable::two_gradual_equivalent_fully_static_types_are_also_equivalent
```

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-05-10 13:10_

---

_Closed by @AlexWaygood on 2025-05-11 09:27_

---
