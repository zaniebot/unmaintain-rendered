```yaml
number: 1171
title: "[panic]"
type: issue
state: closed
author: leclairm
labels: []
assignees: []
created_at: 2025-09-11T15:00:01Z
updated_at: 2025-09-11T15:18:25Z
url: https://github.com/astral-sh/ty/issues/1171
synced_at: 2026-01-12T15:54:24Z
```

# [panic]

---

_@leclairm_

Hi Astral team,

Thanks a lot for your work, fast type checking really helps tremendously!

I was testing `ty` with `uvx ty check` on my code base and got the following error. Just following the suggested feedback procedure.
```
error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/home/matth/Projects/C2SM/Sirocco/standalone/tests/test_sirocco_workflow.py`: `infer_definition_types(Id(4613d)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.20
info: Args: ["ty", "check"]
info: Backtrace:
   0: <unknown>
   1: <unknown>

[...]

 123: <unknown>
 124: <unknown>

info: query stacktrace:
   0: place_by_id(Id(b4ab))
             at crates/ty_python_semantic/src/place.rs:702
   1: Type < 'db >::member_lookup_with_policy_(Id(a105))
             at crates/ty_python_semantic/src/types.rs:715
   2: infer_definition_types(Id(4408a))
             at crates/ty_python_semantic/src/types/infer.rs:174
   3: infer_deferred_types(Id(4417e))
             at crates/ty_python_semantic/src/types/infer.rs:214
   4: ClassLiteral < 'db >::explicit_bases_(Id(1105f))
             at crates/ty_python_semantic/src/types/class.rs:1377
   5: ClassLiteral < 'db >::is_typed_dict_(Id(1105f))
             at crates/ty_python_semantic/src/types/class.rs:1377
   6: infer_deferred_types(Id(3555b))
             at crates/ty_python_semantic/src/types/infer.rs:214
   7: FunctionType < 'db >::signature_(Id(14860))
             at crates/ty_python_semantic/src/types/function.rs:645
   8: infer_expression_types(Id(7bec))
             at crates/ty_python_semantic/src/types/infer.rs:255
   9: infer_definition_types(Id(3fb8c))
             at crates/ty_python_semantic/src/types/infer.rs:174
  10: infer_scope_types(Id(1f19c))
             at crates/ty_python_semantic/src/types/infer.rs:145
  11: check_file_impl(Id(c08))
             at crates/ty_project/src/lib.rs:522


error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/home/matth/Projects/C2SM/Sirocco/standalone/src/sirocco/cli.py`: `infer_definition_types(Id(4613d)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.20
info: Args: ["ty", "check"]
info: Backtrace:
   0: <unknown>
   1: <unknown>

[...]

 191: <unknown>
 192: <unknown>

info: query stacktrace:
   0: place_by_id(Id(b4ab))
             at crates/ty_python_semantic/src/place.rs:702
   1: Type < 'db >::member_lookup_with_policy_(Id(a105))
             at crates/ty_python_semantic/src/types.rs:715
   2: infer_definition_types(Id(4408a))
             at crates/ty_python_semantic/src/types/infer.rs:174
   3: infer_deferred_types(Id(4417e))
             at crates/ty_python_semantic/src/types/infer.rs:214
   4: ClassLiteral < 'db >::explicit_bases_(Id(1105f))
             at crates/ty_python_semantic/src/types/class.rs:1377
   5: ClassLiteral < 'db >::inheritance_cycle_(Id(13c2d))
             at crates/ty_python_semantic/src/types/class.rs:1377
   6: ClassLiteral < 'db >::try_metaclass_(Id(13c2d))
             at crates/ty_python_semantic/src/types/class.rs:1377
   7: infer_definition_types(Id(3545c))
             at crates/ty_python_semantic/src/types/infer.rs:174
   8: place_by_id(Id(11c8e))
             at crates/ty_python_semantic/src/place.rs:702
   9: infer_deferred_types(Id(354a0))
             at crates/ty_python_semantic/src/types/infer.rs:214
  10: infer_scope_types(Id(3702))
             at crates/ty_python_semantic/src/types/infer.rs:145
  11: infer_definition_types(Id(35473))
             at crates/ty_python_semantic/src/types/infer.rs:174
  12: infer_expression_types(Id(4711))
             at crates/ty_python_semantic/src/types/infer.rs:255
  13: infer_expression_type(Id(4711))
             at crates/ty_python_semantic/src/types/infer.rs:313
  14: ClassLiteral < 'db >::implicit_attribute_inner_(Id(27875))
             at crates/ty_python_semantic/src/types/class.rs:1377
  15: Type < 'db >::member_lookup_with_policy_(Id(1a46d))
             at crates/ty_python_semantic/src/types.rs:715
  16: infer_expression_types(Id(3cd55))
             at crates/ty_python_semantic/src/types/infer.rs:255
  17: infer_scope_types(Id(3fcfa))
             at crates/ty_python_semantic/src/types/infer.rs:145
  18: check_file_impl(Id(2001))
             at crates/ty_project/src/lib.rs:522
```



---

_Comment by @carljm on 2025-09-11 15:18_

Thanks for the report!

---

_Closed by @carljm on 2025-09-11 15:18_

---
