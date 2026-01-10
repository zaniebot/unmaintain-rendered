```yaml
number: 21594
title: "[ty] Add failing mdtest for known `Protocol` panic"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/panicking-mdtest
created_at: 2025-11-23T16:02:07Z
updated_at: 2025-11-24T08:21:17Z
url: https://github.com/astral-sh/ruff/pull/21594
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Add failing mdtest for known `Protocol` panic

---

_Pull request opened by @AlexWaygood on 2025-11-23 16:02_

## Summary

This PR adds a failing mdtest for the panic in https://github.com/astral-sh/ty/issues/1587. The added snippet currently panics with this query stacktrace:

```
error[panic]: Panicked at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/17bc55d/src/function/execute.rs:321:21 when checking `/Users/alexw/dev/ruff/foo.py`: `ClassLiteral < 'db >::explicit_bases_(Id(4c09)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Version: ruff/0.14.5+105 (d24c891a4 2025-11-22)
info: Args: ["target/debug/ty", "check", "foo.py", "--python-version=3.14"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: cached_protocol_interface(Id(6805))
             at crates/ty_python_semantic/src/types/protocol_class.rs:795
   1: is_equivalent_to_object_inner(Id(8003))
             at crates/ty_python_semantic/src/types/instance.rs:667
   2: infer_deferred_types(Id(1406))
             at crates/ty_python_semantic/src/types/infer.rs:140
             cycle heads: infer_definition_types(Id(140b)) -> iteration = 200, TypeVarInstance < 'db >::lazy_bound_(Id(5802)) -> iteration = 200
   3: TypeVarInstance < 'db >::lazy_bound_(Id(5803))
             at crates/ty_python_semantic/src/types.rs:8827
   4: infer_definition_types(Id(140c))
             at crates/ty_python_semantic/src/types/infer.rs:94
   5: infer_deferred_types(Id(1405))
             at crates/ty_python_semantic/src/types/infer.rs:140
   6: TypeVarInstance < 'db >::lazy_bound_(Id(5802))
             at crates/ty_python_semantic/src/types.rs:8827
   7: infer_definition_types(Id(140b))
             at crates/ty_python_semantic/src/types/infer.rs:94
   8: infer_scope_types(Id(1000))
             at crates/ty_python_semantic/src/types/infer.rs:70
   9: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:535
```

It's not totally clear to me how to fix this or to what extent it might be a bug in our `Protocol` internals rather than a bug in our `TypeVar` internals. (It's sort of interesting that we're trying to evaluate the upper bound of any `TypeVar`s here!) @carljm suggested that it would be a good idea to add a failing mdtest in the meantime to document the panic, which I agree with.

## Test Plan

I verified that we panic on this snippet, and that the test fails if I remove the `expect-panic` assertion or if I change the asserted error message.

I experimented with ways of minimizing the snippet further, but I think any further minimization takes the snippet further away from something a user would actually be likely to write -- so I think is probably counterproductive. The failing test added in this PR isn't unreasonable code at the end of the day; I've seen Python like it in the wild.


---

_Review requested from @carljm by @AlexWaygood on 2025-11-23 16:02_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-23 16:02_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-23 16:02_

---

_Label `testing` added by @AlexWaygood on 2025-11-23 16:02_

---

_Label `ty` added by @AlexWaygood on 2025-11-23 16:02_

---

_@MichaReiser approved on 2025-11-24 08:08_

---

_Merged by @AlexWaygood on 2025-11-24 08:21_

---

_Closed by @AlexWaygood on 2025-11-24 08:21_

---

_Branch deleted on 2025-11-24 08:21_

---
