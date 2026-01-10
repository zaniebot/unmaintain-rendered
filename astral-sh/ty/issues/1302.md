```yaml
number: 1302
title: "[panic]"
type: issue
state: closed
author: sector119
labels:
  - fatal
assignees: []
created_at: 2025-10-03T16:21:52Z
updated_at: 2025-10-03T16:23:36Z
url: https://github.com/astral-sh/ty/issues/1302
synced_at: 2026-01-10T02:06:25Z
```

# [panic]

---

_Issue opened by @sector119 on 2025-10-03 16:21_

```
error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/3713cd7/src/function/execute.rs:213:25 when checking `/Users/sector119/PycharmProjects/epsilon_xmlrpc/tests/integration/eps/test_dictionary.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(4000c)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Version: 0.0.1-alpha.21 (ef52a1940 2025-09-19)
info: Args: ["ty", "check"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: Type < 'db >::apply_specialization_(Id(3d832))
             at crates/ty_python_semantic/src/types.rs:768
   1: PEP695TypeAliasType < 'db >::value_type_(Id(40009))
             at crates/ty_python_semantic/src/types.rs:9779
   2: TupleType < 'db >::to_class_type_(Id(1f012))
             at crates/ty_python_semantic/src/types/tuple.rs:149
             cycle heads: PEP695TypeAliasType < 'db >::value_type_(Id(4000a)) -> IterationCount(0)
   3: Type < 'db >::apply_specialization_(Id(3d830))
             at crates/ty_python_semantic/src/types.rs:768
   4: PEP695TypeAliasType < 'db >::value_type_(Id(4000a))
             at crates/ty_python_semantic/src/types.rs:9779
   5: Type < 'db >::apply_specialization_(Id(3d81b))
             at crates/ty_python_semantic/src/types.rs:768
   6: PEP695TypeAliasType < 'db >::value_type_(Id(40004))
             at crates/ty_python_semantic/src/types.rs:9779
   7: FunctionType < 'db >::signature_(Id(1cc2f))
             at crates/ty_python_semantic/src/types/function.rs:715
   8: infer_expression_types_impl(Id(4802))
             at crates/ty_python_semantic/src/types/infer.rs:192
   9: infer_definition_types(Id(3807))
             at crates/ty_python_semantic/src/types/infer.rs:97
  10: infer_scope_types(Id(2c02))
             at crates/ty_python_semantic/src/types/infer.rs:68
  11: check_file_impl(Id(c43))
             at crates/ty_project/src/lib.rs:522
```

---

_Comment by @AlexWaygood on 2025-10-03 16:23_

Thanks! Looks like another instance of #256

---

_Closed by @AlexWaygood on 2025-10-03 16:23_

---

_Label `fatal` added by @AlexWaygood on 2025-10-03 16:23_

---
