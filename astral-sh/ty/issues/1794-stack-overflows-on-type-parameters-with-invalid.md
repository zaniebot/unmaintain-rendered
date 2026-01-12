```yaml
number: 1794
title: Stack overflows on type parameters with invalid recursive bounds
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - fatal
  - generics
  - fuzzer
assignees: []
created_at: 2025-12-07T15:28:08Z
updated_at: 2025-12-12T23:49:22Z
url: https://github.com/astral-sh/ty/issues/1794
synced_at: 2026-01-12T15:54:25Z
```

# Stack overflows on type parameters with invalid recursive bounds

---

_@AlexWaygood_

### Summary

Ty currently overflows its stack trying to analyse this snippet:

```py
class name_1[name_2: name_2[0]]:
    def name_4(name_3: name_2, /):
        if name_3:
            pass
```

and also on this, which looks similar:

```py
def name_4[name_5: (name_5 if unique_name_0 else name_1)[0], **name_1](): ...
```

---

_Label `bug` added by @AlexWaygood on 2025-12-07 15:28_

---

_Label `fatal` added by @AlexWaygood on 2025-12-07 15:28_

---

_Label `generics` added by @AlexWaygood on 2025-12-07 15:28_

---

_Label `fuzzer` added by @AlexWaygood on 2025-12-07 15:28_

---

_Renamed from "Stack overflow on type parameter with recursive bounds" to "Stack overflow on type parameters with invalid recursive bounds" by @AlexWaygood on 2025-12-07 15:30_

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-07 15:39_

---

_Comment by @AlexWaygood on 2025-12-07 15:46_

Running under LLDB, the looping part of the stack trace for the first of these appears that it's just going into infinitely nested `try_bool_impl` calls:

```
frame #4150: 0x0000000100ee1514 ty`ty_python_semantic::types::Type::try_bool_impl::hec42848fbb378a1f(self=0x0000000170c5b130, db=&dyn ty_python_semantic::db::Db @ 0x0000000170c5aa20, allow_short_circuit=false, visitor=0x0000000170dfc5f0) at types.rs:5408:31
frame #4151: 0x0000000100ee1514 ty`ty_python_semantic::types::Type::try_bool_impl::hec42848fbb378a1f(self=0x0000000170c5bf60, db=&dyn ty_python_semantic::db::Db @ 0x0000000170c5b850, allow_short_circuit=false, visitor=0x0000000170dfc5f0) at types.rs:5408:31
frame #4152: 0x0000000100ee1514 ty`ty_python_semantic::types::Type::try_bool_impl::hec42848fbb378a1f(self=0x0000000170c5cd90, db=&dyn ty_python_semantic::db::Db @ 0x0000000170c5c680, allow_short_circuit=false, visitor=0x0000000170dfc5f0) at types.rs:5408:31
frame #4153: 0x0000000100ee1514 ty`ty_python_semantic::types::Type::try_bool_impl::hec42848fbb378a1f(self=0x0000000170c5dbc0, db=&dyn ty_python_semantic::db::Db @ 0x0000000170c5d4b0, allow_short_circuit=false, visitor=0x0000000170dfc5f0) at types.rs:5408:31
frame #4154: 0x0000000100ee1514 ty`ty_python_semantic::types::Type::try_bool_impl::hec42848fbb378a1f(self=0x0000000170c5e9f0, db=&dyn ty_python_semantic::db::Db @ 0x0000000170c5e2e0, allow_short_circuit=false, visitor=0x0000000170dfc5f0) at types.rs:5408:31
frame #4155: 0x0000000100ee1514 ty`ty_python_semantic::types::Type::try_bool_impl::hec42848fbb378a1f(self=0x0000000170c5f820, db=&dyn ty_python_semantic::db::Db @ 0x0000000170c5f110, allow_short_circuit=false, visitor=0x0000000170dfc5f0) at types.rs:5408:31
```

the looping part of the stack trace for the second one is:

```
frame #3478: 0x0000000100a94a30 ty`_$LT$I$u20$as$u20$ty_python_semantic..types..constraints..IteratorConstraintsExtension$LT$T$GT$$GT$::when_all::hdcf56949a68b74b2(self=Iter<ty_python_semantic::types::Type> @ 0x0000000170d9a348, db=&dyn ty_python_semantic::db::Db @ 0x0000000170d9a358, f={closure_env#10} @ 0x0000000170d9ace8) at constraints.rs:163:37
frame #3479: 0x0000000100ee9e88 ty`ty_python_semantic::types::Type::has_relation_to_impl::h59999ac21039d235(self=Type @ 0x0000000170d9e170, db=&dyn ty_python_semantic::db::Db @ 0x0000000170d9ba40, target=Type @ 0x0000000170d9e190, inferable=InferableTypeVars @ 0x0000000170d9e1b0, relation=TypeRelation @ 0x0000000170d9e1d0, relation_visitor=0x0000000170df1590, disjointness_visitor=0x0000000170df1608) at types.rs:2251:66
frame #3480: 0x0000000100ee9c38 ty`ty_python_semantic::types::Type::has_relation_to_impl::h59999ac21039d235(self=Type @ 0x0000000170d9ebf0, db=&dyn ty_python_semantic::db::Db @ 0x0000000170d9de30, target=Type @ 0x0000000170d9eb90, inferable=InferableTypeVars @ 0x0000000170d9ebb0, relation=TypeRelation @ 0x0000000170d9ebd0, relation_visitor=0x0000000170df1590, disjointness_visitor=0x0000000170df1608) at types.rs:2173:26
frame #3481: 0x0000000100b381f4 ty`ty_python_semantic::types::Type::has_relation_to_impl::_$u7b$$u7b$closure$u7d$$u7d$::he95108abad2046db((null)=0x0000600003ff8340) at types.rs:2252:25
frame #3482: 0x0000000100a94a30 ty`_$LT$I$u20$as$u20$ty_python_semantic..types..constraints..IteratorConstraintsExtension$LT$T$GT$$GT$::when_all::hdcf56949a68b74b2(self=Iter<ty_python_semantic::types::Type> @ 0x0000000170d9ecb8, db=&dyn ty_python_semantic::db::Db @ 0x0000000170d9ecc8, f={closure_env#10} @ 0x0000000170d9f658) at constraints.rs:163:37
```

---

_Renamed from "Stack overflow on type parameters with invalid recursive bounds" to "Stack overflows on type parameters with invalid recursive bounds" by @AlexWaygood on 2025-12-07 15:56_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-12-07 18:06_

---

_Unassigned @AlexWaygood by @AlexWaygood on 2025-12-12 08:17_

---

_Comment by @AlexWaygood on 2025-12-12 08:17_

Both fixed in #21840

---

_Closed by @AlexWaygood on 2025-12-12 08:17_

---

_Comment by @dhruvmanila on 2025-12-12 08:40_

I'm still seeing stack overflow on `main` (https://github.com/astral-sh/ruff/commit/0138cd238a8669d53c325eb7ca19155946dfa665) for the first example (no stack overflow for the second example) assuming that by #21840 you meant astral-sh/ruff#21840.

---

_Comment by @AlexWaygood on 2025-12-12 08:48_

> assuming that by #21840 you meant [astral-sh/ruff#21840](https://github.com/astral-sh/ruff/pull/21840).

I did, yes — thanks!

Hmm, https://github.com/astral-sh/ruff/pull/21840 added some regression tests that should have covered this but maybe there was a bug in one of the tests?

---

_Reopened by @AlexWaygood on 2025-12-12 08:48_

---

_Comment by @mtshiba on 2025-12-12 09:11_

The diff between the panicking version and astral-sh/ruff#21840 is below.

```diff
- class _[T: (0, T[0])]:
+ class _[T: T[0]]:
    def _(x: T):
        if x:
            pass
```

The issue seems to be solved if we also prohibit bad specialization of type variables themselves.


---

_Closed by @carljm on 2025-12-12 23:49_

---
