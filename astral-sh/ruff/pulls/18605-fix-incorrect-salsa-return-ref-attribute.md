```yaml
number: 18605
title: "Fix incorrect salsa `return_ref` attribute"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/fix-incorrect-salsa-attr
created_at: 2025-06-10T07:06:56Z
updated_at: 2025-06-11T07:19:59Z
url: https://github.com/astral-sh/ruff/pull/18605
synced_at: 2026-01-12T15:56:22Z
```

# Fix incorrect salsa `return_ref` attribute

---

_@MichaReiser_

## Summary

Update salsa to now get warned about unknown attributes on salsa-structs and documented getters.

## Test Plan

`cargo build`


---

_Review requested from @carljm by @MichaReiser on 2025-06-10 07:06_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-10 07:06_

---

_Review requested from @sharkdp by @MichaReiser on 2025-06-10 07:06_

---

_Review requested from @dcreager by @MichaReiser on 2025-06-10 07:06_

---

_Label `internal` added by @MichaReiser on 2025-06-10 07:07_

---

_Comment by @github-actions[bot] on 2025-06-10 07:10_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @MichaReiser on 2025-06-10 07:11_

@ibraheemdev I'm not sure if this is the same as https://github.com/salsa-rs/salsa/issues/904 but many corpus tests are failing with 

```
thread 'typeshed_no_panic' panicked at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/8924db1/src/interned.rs:698:9:
Data was not interned in the latest revision for its durability.
```

---

_Comment by @AlexWaygood on 2025-06-10 07:18_

Also one mdtest:

```
descriptor_protocol.… - Descriptor protocol - Basic properties - Descriptors only wor… (82302db0798c7f28)

      crates/ty_python_semantic/resources/mdtest/descriptor_protocol.md:201 panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/8924db1/src/interned.rs:698:9
      crates/ty_python_semantic/resources/mdtest/descriptor_protocol.md:201 Data was not interned in the latest revision for its durability.
      crates/ty_python_semantic/resources/mdtest/descriptor_protocol.md:201 run with `RUST_BACKTRACE=1` environment variable to display a backtrace
      crates/ty_python_semantic/resources/mdtest/descriptor_protocol.md:201 query stacktrace:
      crates/ty_python_semantic/resources/mdtest/descriptor_protocol.md:201    0: infer_expression_types(Id(1001)) -> (R24, Durability::LOW)
      crates/ty_python_semantic/resources/mdtest/descriptor_protocol.md:201              at crates/ty_python_semantic/src/types/infer.rs:240
      crates/ty_python_semantic/resources/mdtest/descriptor_protocol.md:201    1: infer_scope_types(Id(800)) -> (R24, Durability::LOW)
      crates/ty_python_semantic/resources/mdtest/descriptor_protocol.md:201              at crates/ty_python_semantic/src/types/infer.rs:135
      crates/ty_python_semantic/resources/mdtest/descriptor_protocol.md:201    2: check_types(Id(0)) -> (R24, Durability::LOW)
      crates/ty_python_semantic/resources/mdtest/descriptor_protocol.md:201              at crates/ty_python_semantic/src/types.rs:88
      crates/ty_python_semantic/resources/mdtest/descriptor_protocol.md:201 

    To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER='descriptor_protocol.… - Descriptor protocol - Basic properties - Descriptors only wor… (82302db0798c7f28)'
    MDTEST_TEST_FILTER='descriptor_protocol.… - Descriptor protocol - Basic properties - Descriptors only wor… (82302db0798c7f28)' cargo test -p ty_python_semantic --test mdtest -- mdtest__descriptor_protocol

    --------------------------------------------------

    test mdtest__descriptor_protocol ... FAILED

    failures:

    failures:
        mdtest__descriptor_protocol
```

---

_Label `ty` added by @AlexWaygood on 2025-06-10 08:45_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-10 15:53_

---

_@AlexWaygood approved on 2025-06-11 07:19_

---

_Comment by @github-actions[bot] on 2025-06-11 07:19_

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

_Merged by @MichaReiser on 2025-06-11 07:19_

---

_Closed by @MichaReiser on 2025-06-11 07:19_

---

_Branch deleted on 2025-06-11 07:19_

---
