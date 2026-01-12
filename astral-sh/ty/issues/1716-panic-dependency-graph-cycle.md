```yaml
number: 1716
title: "[panic] dependency graph cycle"
type: issue
state: closed
author: Matt-Ord
labels:
  - fatal
assignees: []
created_at: 2025-12-02T09:18:38Z
updated_at: 2025-12-03T19:01:43Z
url: https://github.com/astral-sh/ty/issues/1716
synced_at: 2026-01-12T15:54:25Z
```

# [panic] dependency graph cycle

---

_@Matt-Ord_

Not sure if you expect to have fixed all of these issues yet - but thought I would submit a ticket

when type checking
https://github.com/Matt-Ord/slate

```
ty check ./slate_core/array/_misc.py
```

panics with

```
$ ty check ./slate_core/array/_misc.py
error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/17bc55d/src/function/fetch.rs:227:21 when checking `/workspaces/slate/slate_core/array/_misc.py`: `dependency graph cycle when querying GenericAlias < 'db >::variance_of_(Id(d806)), set cycle_fn/cycle_initial to fixpoint iterate.
Query stack:
[
    check_file_impl(Id(c00)),
    infer_scope_types(Id(1001)),
    PEP695TypeAliasType < 'db >::raw_value_type_(Id(8002)),
    infer_scope_types(Id(7cb4)),
    Type < 'db >::member_lookup_with_policy_(Id(2c41)),
    Type < 'db >::try_call_dunder_get_(Id(10000)),
    ClassLiteral < 'db >::variance_of_(Id(c804)),
    GenericAlias < 'db >::variance_of_(Id(d806)),
    ClassLiteral < 'db >::variance_of_(Id(c806)),
    infer_expression_types_impl(Id(1a67)),
    infer_definition_types(Id(843d)),
    FunctionType < 'db >::signature_(Id(403a)),
    FunctionType < 'db >::signature_(Id(403b)),
    infer_scope_types(Id(7cdf)),
    TypeVarInstance < 'db >::lazy_bound_(Id(8c22)),
    infer_deferred_types(Id(8436)),
    infer_definition_types(Id(842d)),
    place_by_id(Id(3465)),
    infer_definition_types(Id(1717)),
    Type < 'db >::member_lookup_with_policy_(Id(2c37)),
    place_by_id(Id(345f)),
    infer_definition_types(Id(15c8)),
    infer_definition_types(Id(bd46)),
    infer_definition_types(Id(bd44)),
    Type < 'db >::member_lookup_with_policy_(Id(2c32)),
    place_by_id(Id(3459)),
    infer_definition_types(Id(4d24)),
    Type < 'db >::member_lookup_with_policy_(Id(2c33)),
    place_by_id(Id(345a)),
    infer_definition_types(Id(bdc8)),
    file_to_module(Id(c86)),
    Type < 'db >::member_lookup_with_policy_(Id(2c34)),
    imported_modules(Id(c86)),
]`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.29
info: Args: ["ty", "check", "./slate_core/array/_misc.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: GenericAlias < 'db >::variance_of_(Id(d806))
             at crates/ty_python_semantic/src/types/class.rs:344
   1: ClassLiteral < 'db >::variance_of_(Id(c804))
             at crates/ty_python_semantic/src/types/class.rs:3782
   2: Type < 'db >::try_call_dunder_get_(Id(10000))
             at crates/ty_python_semantic/src/types.rs:873
   3: Type < 'db >::member_lookup_with_policy_(Id(2c41))
             at crates/ty_python_semantic/src/types.rs:873
   4: infer_scope_types(Id(7cb4))
             at crates/ty_python_semantic/src/types/infer.rs:68
   5: PEP695TypeAliasType < 'db >::raw_value_type_(Id(8002))
             at crates/ty_python_semantic/src/types.rs:12712
   6: infer_scope_types(Id(1001))
             at crates/ty_python_semantic/src/types/infer.rs:68
   7: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:535


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

---

_Label `fatal` added by @AlexWaygood on 2025-12-02 09:21_

---

_Added to milestone `Beta` by @MichaReiser on 2025-12-02 09:23_

---

_Comment by @AlexWaygood on 2025-12-03 00:02_

This fixes the panic and passes all existing tests, but I haven't tried to find a minimal repro for a regression test yet:

```diff
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index 1285fb0093..712bb8806a 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -340,9 +340,18 @@ impl<'db> From<GenericAlias<'db>> for Type<'db> {
     }
 }
 
+fn variance_of_cycle_initial<'db>(
+    _db: &'db dyn Db,
+    _id: salsa::Id,
+    _self: GenericAlias<'db>,
+    _typevar: BoundTypeVarInstance<'db>,
+) -> TypeVarVariance {
+    TypeVarVariance::Bivariant
+}
+
 #[salsa::tracked]
 impl<'db> VarianceInferable<'db> for GenericAlias<'db> {
-    #[salsa::tracked(heap_size=ruff_memory_usage::heap_size)]
+    #[salsa::tracked(heap_size=ruff_memory_usage::heap_size, cycle_initial=variance_of_cycle_initial)]
     fn variance_of(self, db: &'db dyn Db, typevar: BoundTypeVarInstance<'db>) -> TypeVarVariance {
         let origin = self.origin(db);
```

---

_Comment by @carljm on 2025-12-03 00:04_

Yeah that looks like the right fix here, I expected we just needed a cycle handler for that query.

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-12-03 17:47_

---

_Comment by @AlexWaygood on 2025-12-03 18:24_

Okay, here's a self-contained repro:

```py
from typing import Protocol

class A(Protocol):
    @property
    def f(self): ...

type Recursive = int | tuple[Recursive, ...]

class B[T: A]: ...

class C[T: A](A):
    x: tuple[Recursive, ...]

class D(B[C]): ...
```

---

_Closed by @AlexWaygood on 2025-12-03 19:01_

---
