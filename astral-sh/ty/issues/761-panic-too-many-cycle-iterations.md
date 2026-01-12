```yaml
number: 761
title: "[panic] too many cycle iterations"
type: issue
state: closed
author: bugzpodder
labels: []
assignees: []
created_at: 2025-07-03T23:16:52Z
updated_at: 2025-07-04T01:29:47Z
url: https://github.com/astral-sh/ty/issues/761
synced_at: 2026-01-12T15:54:23Z
```

# [panic] too many cycle iterations

---

_@bugzpodder_

```
error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/fc00eba/src/function/execute.rs:212:25 when checking `/Users/bugzpodder/dev/shipping_test.py`: `infer_definition_types(Id(69be7)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Args: ["ty", "check", "--python-version", "3.11", "dev"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_deferred_types(Id(69bfb))
             at crates/ty_python_semantic/src/types/infer.rs:198
   1: ClassLiteral < 'db >::explicit_bases_(Id(2284d))
             at crates/ty_python_semantic/src/types/class.rs:788
   2: infer_deferred_types(Id(1785))
             at crates/ty_python_semantic/src/types/infer.rs:198
   3: FunctionType < 'db >::signature_(Id(1f884))
             at crates/ty_python_semantic/src/types/function.rs:573
   4: infer_expression_types(Id(5afd3))
             at crates/ty_python_semantic/src/types/infer.rs:235
   5: infer_definition_types(Id(69b3d))
             at crates/ty_python_semantic/src/types/infer.rs:159
   6: infer_scope_types(Id(6ac9a))
             at crates/ty_python_semantic/src/types/infer.rs:130
   7: check_types(Id(c1b))
             at crates/ty_python_semantic/src/types.rs:89


error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/fc00eba/src/function/execute.rs:212:25 when checking `/Users/bugzpodder/dev/denormalization.py`: `infer_definition_types(Id(69be7)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Args: ["ty", "check", "--python-version", "3.11", "dev"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: place_by_id(Id(16cf5))
             at crates/ty_python_semantic/src/place.rs:623
   1: Type < 'db >::member_lookup_with_policy_(Id(c1a9))
             at crates/ty_python_semantic/src/types.rs:583
   2: infer_definition_types(Id(16b4))
             at crates/ty_python_semantic/src/types/infer.rs:159
   3: place_by_id(Id(16cf4))
             at crates/ty_python_semantic/src/place.rs:623
   4: Type < 'db >::member_lookup_with_policy_(Id(c1a8))
             at crates/ty_python_semantic/src/types.rs:583
   5: infer_definition_types(Id(730bf))
             at crates/ty_python_semantic/src/types/infer.rs:159
   6: infer_scope_types(Id(6acd0))
             at crates/ty_python_semantic/src/types/infer.rs:130
   7: check_types(Id(cfc))
             at crates/ty_python_semantic/src/types.rs:89


error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/fc00eba/src/function/execute.rs:212:25 when checking `/Users/bugzpodder/dev/logical_pipeline_plan.py`: `infer_definition_types(Id(69be7)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Args: ["ty", "check", "--python-version", "3.11", "dev"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_deferred_types(Id(69bfb))
             at crates/ty_python_semantic/src/types/infer.rs:198
   1: ClassLiteral < 'db >::explicit_bases_(Id(2284d))
             at crates/ty_python_semantic/src/types/class.rs:788
   2: infer_deferred_types(Id(1785))
             at crates/ty_python_semantic/src/types/infer.rs:198
   3: FunctionType < 'db >::signature_(Id(1f884))
             at crates/ty_python_semantic/src/types/function.rs:573
   4: infer_expression_types(Id(3ec2b))
             at crates/ty_python_semantic/src/types/infer.rs:235
   5: infer_definition_types(Id(3c61e))
             at crates/ty_python_semantic/src/types/infer.rs:159
   6: infer_scope_types(Id(1cfb))
             at crates/ty_python_semantic/src/types/infer.rs:130
   7: check_types(Id(cd3))
             at crates/ty_python_semantic/src/types.rs:89


error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/fc00eba/src/function/execute.rs:212:25 when checking `/Users/bugzpodder/dev/graph.py`: `infer_definition_types(Id(69be7)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Args: ["ty", "check", "--python-version", "3.11", "dev"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: place_by_id(Id(16cf5))
             at crates/ty_python_semantic/src/place.rs:623
   1: Type < 'db >::member_lookup_with_policy_(Id(c1a9))
             at crates/ty_python_semantic/src/types.rs:583
   2: infer_definition_types(Id(16b4))
             at crates/ty_python_semantic/src/types/infer.rs:159
   3: infer_scope_types(Id(10bd))
             at crates/ty_python_semantic/src/types/infer.rs:130
   4: check_types(Id(c20))
             at crates/ty_python_semantic/src/types.rs:89


error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/fc00eba/src/function/execute.rs:212:25 when checking `/Users/bugzpodder/dev/denormalization.py`: `infer_definition_types(Id(69be7)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Args: ["ty", "check", "--python-version", "3.11", "dev"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_deferred_types(Id(69bfb))
             at crates/ty_python_semantic/src/types/infer.rs:198
   1: ClassLiteral < 'db >::explicit_bases_(Id(2284d))
             at crates/ty_python_semantic/src/types/class.rs:788
   2: infer_deferred_types(Id(1792))
             at crates/ty_python_semantic/src/types/infer.rs:198
   3: FunctionType < 'db >::signature_(Id(1b92d))
             at crates/ty_python_semantic/src/types/function.rs:573
   4: infer_expression_types(Id(3edc0))
             at crates/ty_python_semantic/src/types/infer.rs:235
   5: infer_definition_types(Id(44298))
             at crates/ty_python_semantic/src/types/infer.rs:159
   6: infer_scope_types(Id(1d98))
             at crates/ty_python_semantic/src/types/infer.rs:130
   7: check_types(Id(cbe))
             at crates/ty_python_semantic/src/types.rs:89
```

---

_Comment by @carljm on 2025-07-04 01:29_

Thanks for the report! It's hard to say for sure without seeing the code in question, but this looks like the same issue as #256 , so I'm going to close it as a duplicate.

---

_Closed by @carljm on 2025-07-04 01:29_

---
