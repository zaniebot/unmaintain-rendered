```yaml
number: 1058
title: "[panic] execute: too many cycle iterations"
type: issue
state: closed
author: aivarannamaa
labels:
  - needs-mre
  - fatal
assignees: []
created_at: 2025-08-20T05:38:57Z
updated_at: 2025-09-11T14:06:18Z
url: https://github.com/astral-sh/ty/issues/1058
synced_at: 2026-01-12T15:54:24Z
```

# [panic] execute: too many cycle iterations

---

_@aivarannamaa_

Got following stacktrace for `ty check`:

```
Checking ------------------------------------------------------------ 19/19 files
error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/a/secret/fle.py`: `infer_definition_types(Id(3f5b6)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Version: 0.0.1-alpha.19 (e9cb838b3 2025-08-19)
info: Args: ["ty", "check"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: place_by_id(Id(1643c))
             at crates/ty_python_semantic/src/place.rs:688
   1: infer_definition_types(Id(3f5bb))
             at crates/ty_python_semantic/src/types/infer.rs:167
   2: ClassLiteral < 'db >::try_mro_(Id(43814))
             at crates/ty_python_semantic/src/types/class.rs:1230
   3: ClassLiteral < 'db >::is_typed_dict_(Id(3c00b))
             at crates/ty_python_semantic/src/types/class.rs:1230
   4: infer_deferred_types(Id(3f5be))
             at crates/ty_python_semantic/src/types/infer.rs:207
   5: FunctionType < 'db >::signature_(Id(3902b))
             at crates/ty_python_semantic/src/types/function.rs:642
   6: infer_expression_types(Id(11c7d))
             at crates/ty_python_semantic/src/types/infer.rs:248
   7: infer_definition_types(Id(4f08))
             at crates/ty_python_semantic/src/types/infer.rs:167
   8: infer_expression_types(Id(11c7e))
             at crates/ty_python_semantic/src/types/infer.rs:248
   9: infer_definition_types(Id(4f0a))
             at crates/ty_python_semantic/src/types/infer.rs:167
  10: ClassLiteral < 'db >::implicit_attribute_inner_(Id(4f007))
             at crates/ty_python_semantic/src/types/class.rs:1230
  11: Type < 'db >::member_lookup_with_policy_(Id(1ec3f))
             at crates/ty_python_semantic/src/types.rs:635
  12: infer_expression_types(Id(78b1))
             at crates/ty_python_semantic/src/types/infer.rs:248
  13: infer_definition_types(Id(64c2))
             at crates/ty_python_semantic/src/types/infer.rs:167
  14: infer_scope_types(Id(3818))
             at crates/ty_python_semantic/src/types/infer.rs:138
  15: check_file_impl(Id(c09))
             at crates/ty_project/src/lib.rs:527
```

Unfortunately I can't share my code.

---

_Label `needs-mre` added by @carljm on 2025-08-20 18:08_

---

_Comment by @carljm on 2025-08-20 18:08_

We have some known causes of this, will try to comment here when some of those are fixed so you can see if it resolves the issue on your code.

---

_Label `fatal` added by @carljm on 2025-08-20 18:09_

---

_Comment by @konstin on 2025-09-11 08:38_

We're seeing the same in https://github.com/astral-sh/packse:

```
error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/home/konsti/projects/packse/src/packse/view.py`: `infer_definition_types(Id(1b66f)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.20
info: Args: ["ty", "check"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: place_by_id(Id(3c858))
             at crates/ty_python_semantic/src/place.rs:702
   1: infer_definition_types(Id(1b674))
             at crates/ty_python_semantic/src/types/infer.rs:174
   2: ClassLiteral < 'db >::try_mro_(Id(4fc44))
             at crates/ty_python_semantic/src/types/class.rs:1377
             cycle heads: ClassLiteral < 'db >::is_typed_dict_(Id(3d40b)) -> IterationCount(0)
   3: ClassLiteral < 'db >::is_typed_dict_(Id(3d40b))
             at crates/ty_python_semantic/src/types/class.rs:1377
   4: infer_deferred_types(Id(1b677))
             at crates/ty_python_semantic/src/types/infer.rs:214
   5: FunctionType < 'db >::signature_(Id(57044))
             at crates/ty_python_semantic/src/types/function.rs:645
   6: infer_expression_types(Id(10431))
             at crates/ty_python_semantic/src/types/infer.rs:255
   7: infer_definition_types(Id(b847))
             at crates/ty_python_semantic/src/types/infer.rs:174
   8: infer_expression_types(Id(1043a))
             at crates/ty_python_semantic/src/types/infer.rs:255
   9: infer_definition_types(Id(b84c))
             at crates/ty_python_semantic/src/types/infer.rs:174
  10: ClassLiteral < 'db >::implicit_attribute_inner_(Id(77c37))
             at crates/ty_python_semantic/src/types/class.rs:1377
  11: Type < 'db >::member_lookup_with_policy_(Id(3104f))
             at crates/ty_python_semantic/src/types.rs:715
  12: Type < 'db >::member_lookup_with_policy_(Id(3104e))
             at crates/ty_python_semantic/src/types.rs:715
  13: infer_expression_types(Id(20c2d))
             at crates/ty_python_semantic/src/types/infer.rs:255
  14: all_narrowing_constraints_for_expression(Id(20c2d))
             at crates/ty_python_semantic/src/types/narrow.rs:84
  15: infer_scope_types(Id(18405))
             at crates/ty_python_semantic/src/types/infer.rs:145
  16: check_file_impl(Id(1001))
             at crates/ty_project/src/lib.rs:522
```

---

_Comment by @sharkdp on 2025-09-11 09:07_

> We're seeing the same in https://github.com/astral-sh/packse:

Usage of `packaging.requirements.Requirement` is problematic; this is related to #256.

---

_Closed by @carljm on 2025-09-11 14:06_

---
