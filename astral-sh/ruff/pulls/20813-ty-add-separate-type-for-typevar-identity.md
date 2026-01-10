```yaml
number: 20813
title: "[ty] Add separate type for typevar \"identity\""
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dcreager/typevar-identity
created_at: 2025-10-11T19:54:07Z
updated_at: 2025-10-14T00:09:29Z
url: https://github.com/astral-sh/ruff/pull/20813
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Add separate type for typevar "identity"

---

_Pull request opened by @dcreager on 2025-10-11 19:54_

As part of #20598, we added `is_identical_to` methods to `TypeVarInstance` and `BoundTypeVarInstance`, which compare when two typevar instances refer to "the same" underlying typevar, even if we have forced their lazy bounds/constraints as part of marking typevars as inferable. (Doing so results in a different salsa interned struct ID, since we've changed the contents of the `bounds_or_constraints` field.)

It turns out that marking typevars as inferable is not the only way that we might force lazy bounds/constraints; it also happens when we materialize a type containing a typevar. This surfaced as ecosystem report failures on #20677.

That means that we need a more long-term fix to this problem. (`is_identical_to`, and its underlying `original` field, were meant to be a temporary fix until we removed the `MarkTypeVarsInferable` type mapping.)

This PR extracts out a separate type (`TypeVarIdentity`) that only includes the fields that actually inform whether two typevars are "the same". All other properties of the typevar (default, bounds/constraints, etc) still live in `TypeVarInstance`. Call sites that care about typevar identity can now either store just `TypeVarIdentity` (if they never need access to those other properties), or continue to store `TypeVarInstance` but pull out its `identity` when performing those "are they the same typevar" comparisons. (All of this also applies respectively to `BoundTypeVar{Identity,Instance}`.) In particular, constraint sets now work on `BoundTypeVarIdentity`, and generic contexts still _store_ a `BoundTypeVarInstance` (since we might need access to defaults when specializing), but are keyed on `BoundTypeVarIdentity`.

---

_Review requested from @carljm by @dcreager on 2025-10-11 19:54_

---

_Review requested from @AlexWaygood by @dcreager on 2025-10-11 19:54_

---

_Review requested from @sharkdp by @dcreager on 2025-10-11 19:54_

---

_Label `internal` added by @dcreager on 2025-10-11 19:54_

---

_Label `ty` added by @dcreager on 2025-10-11 19:54_

---

_Comment by @github-actions[bot] on 2025-10-11 19:56_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-13 23:49:17.110848042 +0000
+++ new-output.txt	2025-10-13 23:49:20.423865224 +0000
@@ -1,5 +1,5 @@
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(cc17)): execute: too many cycle iterations`
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1603f)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(d017)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1643f)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-10-11 19:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
sympy (https://github.com/sympy/sympy)
- sympy/polys/polyclasses.py:1289:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 11030 diagnostics
+ Found 11029 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Label `ecosystem-analyzer` added by @dcreager on 2025-10-11 20:06_

---

_Comment by @github-actions[bot] on 2025-10-11 20:12_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unused-ignore-comment` | 0 | 1 | 0 |
| **Total** | **0** | **1** | **0** |

**[Full report with detailed diff](https://dcreager-typevar-identity.ecosystem-663.pages.dev/diff)** ([timing results](https://dcreager-typevar-identity.ecosystem-663.pages.dev/timing))


---

_@sharkdp approved on 2025-10-13 08:03_

This looks great, thank you!

It looks like we can now revert @carljm's change in `signatures.rs` and potentially reclaim some performance?

```diff
diff --git a/crates/ty_python_semantic/src/types/signatures.rs b/crates/ty_python_semantic/src/types/signatures.rs
index 039b89a6eb..108df8cb00 100644
--- a/crates/ty_python_semantic/src/types/signatures.rs
+++ b/crates/ty_python_semantic/src/types/signatures.rs
@@ -1292,19 +1292,8 @@ impl<'db> Parameters<'db> {
                     let class = nearest_enclosing_class(db, index, scope_id).unwrap();
 
                     Some(
-                        // It looks like unnecessary work here that we create the implicit Self
-                        // annotation using non-inferable typevars and then immediately apply
-                        // `MarkTypeVarsInferable` to it. However, this is currently necessary to
-                        // ensure that implicit-Self and explicit Self annotations are both treated
-                        // the same. Marking type vars inferable will cause reification of lazy
-                        // typevar defaults/bounds/constraints; this needs to happen for both
-                        // implicit and explicit Self so they remain the "same" typevar.
-                        typing_self(db, scope_id, typevar_binding_context, class, &Type::NonInferableTypeVar)
-                            .expect("We should always find the surrounding class for an implicit self: Self annotation").apply_type_mapping(
-                                db,
-                                &TypeMapping::MarkTypeVarsInferable(None),
-                                TypeContext::default()
-                            )
+                        typing_self(db, scope_id, typevar_binding_context, class, &Type::TypeVar)
+                            .expect("We should always find the surrounding class for an implicit self: Self annotation")
                     )
                 } else {
                     // For methods of non-generic classes that are not otherwise generic (e.g. return `Self` or
```

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-10-13 08:07_

---

_Comment by @dcreager on 2025-10-13 20:58_

> It looks like we can now revert @carljm's change in `signatures.rs` and potentially reclaim some performance?

I currently have that being removed in #20677, where we remove the separate enum variants for inferable vs non-inferable. But I can try it here too and see if everything still passes.

---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-10-13 21:03_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-10-13 21:08_

---

_Comment by @dcreager on 2025-10-13 23:47_

> But I can try it here too and see if everything still passes.

Removing this here added back a `no-matching-overload` ecosystem diagnostic that had been removed. I'm going to keep this removal on #20677, where I think it will be fully redundant.

---

_Merged by @dcreager on 2025-10-14 00:09_

---

_Closed by @dcreager on 2025-10-14 00:09_

---

_Branch deleted on 2025-10-14 00:09_

---
