```yaml
number: 549
title: Daily property test run failed on Fri May 30 2025
type: issue
state: closed
author: github-actions
labels:
  - bug
  - type properties
assignees: []
created_at: 2025-05-30T12:05:15Z
updated_at: 2025-05-30T15:49:21Z
url: https://github.com/astral-sh/ty/issues/549
synced_at: 2026-01-10T02:34:10Z
```

# Daily property test run failed on Fri May 30 2025

---

_Issue opened by @github-actions on 2025-05-30 12:05_

Run listed here: https://github.com/astral-sh/ty/actions/runs/15346325092

---

_Label `bug` added by @github-actions[bot] on 2025-05-30 12:05_

---

_Label `type properties` added by @github-actions[bot] on 2025-05-30 12:05_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-05-30 12:08_

---

_Comment by @AlexWaygood on 2025-05-30 12:08_

```
failures:

---- types::property_tests::stable::disjoint_from_is_irreflexive stdout ----

thread 'types::property_tests::stable::disjoint_from_is_irreflexive' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (Intersection { pos: [KnownClassInstance(SpecialForm), Callable { params: GradualForm, returns: None }], neg: [] })
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

---- types::property_tests::stable::disjoint_from_is_symmetric stdout ----

thread 'types::property_tests::stable::disjoint_from_is_symmetric' panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (Callable { params: List([]), returns: Some(Union([KnownClassInstance(NoDefaultType), BuiltinClassLiteral("str")])) }, KnownClassInstance(Bool))
```

---

_Closed by @AlexWaygood on 2025-05-30 15:49_

---
