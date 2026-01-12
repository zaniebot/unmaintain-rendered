```yaml
number: 20028
title: "[ty] Shrink size of `AstNodeRef`"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: ibraheem/ast-node-ref-size
created_at: 2025-08-21T18:28:25Z
updated_at: 2025-08-22T21:03:24Z
url: https://github.com/astral-sh/ruff/pull/20028
synced_at: 2026-01-12T15:56:53Z
```

# [ty] Shrink size of `AstNodeRef`

---

_@ibraheemdev_

## Summary

Removes the `module_ptr` field from `AstNodeRef` in release mode, and change `NodeIndex` to a `NonZeroU32` to reduce the size of `Option<AstNodeRef<_>>` fields.

I believe CI runs in debug mode, so this won't show up in the memory report, but this reduces memory by ~2% in release mode.

---

_Review requested from @carljm by @ibraheemdev on 2025-08-21 18:28_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-08-21 18:28_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-08-21 18:28_

---

_Review requested from @dcreager by @ibraheemdev on 2025-08-21 18:28_

---

_Review requested from @MichaReiser by @ibraheemdev on 2025-08-21 18:28_

---

_Label `internal` added by @ibraheemdev on 2025-08-21 18:28_

---

_Label `ty` added by @ibraheemdev on 2025-08-21 18:28_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/ast_node_ref.rs`:127 on 2025-08-21 18:28_

I'm not completely sure if this is worth the memory usage improvements, cc @MichaReiser.

---

_@ibraheemdev reviewed on 2025-08-21 18:28_

---

_Comment by @github-actions[bot] on 2025-08-21 18:30_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-21 18:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review requested from @dhruvmanila by @ibraheemdev on 2025-08-21 19:01_

---

_Comment by @github-actions[bot] on 2025-08-21 19:13_

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

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/node_index.rs`:66 on 2025-08-22 06:39_

I was a bit confused by the `Debug` output because it wasn't clear to me that `..` means the same as dummy. I think something like `AtomicNodeIndex::dummy` would be more clear (and also avoids the line break)

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/node_index.rs`:65 on 2025-08-22 06:40_

Should we add a `debug_assertion` here that `value + 1 != dummy` 

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/ast_node_ref.rs`:127 on 2025-08-22 06:43_

It's probably fine because `AstNodeRef` fields are all tracked separately (it's very likely that many indices change when you make a change in code) and
any query depending on an `AstNodeRef` field always depends on the AST too. 

Could we now derive `Update?

---

_@MichaReiser approved on 2025-08-22 06:44_

Nice. Can you try to get some numbers from a real world project when using a release build? I'm curious how much this reduces the memory usage for `Expression` and `Definition` (of which we have many of)

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/ast_node_ref.rs`:127 on 2025-08-22 20:50_

> Could we now derive `Update`

I don't think so, the `Update` derive checks for equality of the fields, which would be incorrect here.

---

_@ibraheemdev reviewed on 2025-08-22 20:50_

---

_Comment by @ibraheemdev on 2025-08-22 21:00_

Here's the diff from on a release build of `prefect`. The effect on `Definition` and `Expression` is quite significant.
```diff
 Found 29228 diagnostics
 =======SALSA STRUCTS=======
-`Definition`                                       metadata=19.83MB  fields=39.66MB  count=495746
-`Expression`                                       metadata=12.75MB  fields=14.87MB  count=265605
+`Definition`                                       metadata=19.83MB  fields=27.76MB  count=495746
+`Expression`                                       metadata=12.75MB  fields=6.37MB   count=265605
 `StringLiteralType`                                metadata=3.24MB   fields=5.88MB   count=57943
 `IntersectionType`                                 metadata=1.99MB   fields=4.60MB   count=35545
 `File`                                             metadata=1.69MB   fields=4.52MB   count=26449
@@ -24,12 +24,12 @@
 `FileModule`                                       metadata=0.11MB   fields=0.29MB   count=1992
 `GenericAlias`                                     metadata=0.91MB   fields=0.26MB   count=16249
 `ModuleNameIngredient`                             metadata=0.24MB   fields=0.23MB   count=4344
-`Unpack`                                           metadata=0.17MB   fields=0.20MB   count=4223
 `ModuleLiteralType`                                metadata=0.44MB   fields=0.17MB   count=8470
 `TypeVarInstance`                                  metadata=0.12MB   fields=0.16MB   count=2176
 `StarImportPlaceholderPredicate`                   metadata=0.22MB   fields=0.16MB   count=7889
 `BoundMethodType`                                  metadata=0.36MB   fields=0.16MB   count=6485
 `GenericContext`                                   metadata=0.10MB   fields=0.15MB   count=1822
+`Unpack`                                           metadata=0.17MB   fields=0.14MB   count=4223
 `BoundTypeVarInstance`                             metadata=0.25MB   fields=0.07MB   count=4513
 `PatternPredicate`                                 metadata=0.02MB   fields=0.07MB   count=768
 `Project`                                          metadata=0.00MB   fields=0.06MB   count=1
@@ -51,7 +51,7 @@
 `module_type_symbols::interned_arguments`          metadata=0.00MB   fields=0.00MB   count=1
 =======SALSA QUERIES=======
 `semantic_index -> ty_python_semantic::semantic_index::SemanticIndex`
-    metadata=21.75MB  fields=409.71MB count=6117
+    metadata=21.75MB  fields=407.52MB count=6117
 `parsed_module -> ruff_db::parsed::ParsedModule`
     metadata=0.47MB   fields=284.40MB count=6126
 `use_def_map -> alloc::sync::Arc<ty_python_semantic::semantic_index::use_def::UseDefMap>`
@@ -157,8 +157,8 @@
 `TypeVarInstance < 'db >::lazy_constraints_ -> core::option::Option<ty_python_semantic::types::TypeVarBoundOrConstraints>`
     metadata=0.00MB   fields=0.00MB   count=3
 =======SALSA SUMMARY=======
-TOTAL MEMORY USAGE: 1420.38MB
+TOTAL MEMORY USAGE: 1397.73MB
     struct metadata = 71.31MB
-    struct fields = 93.10MB
+    struct fields = 72.63MB
     memo metadata = 213.48MB
-    memo fields = 1042.49MB
+    memo fields = 1040.30MB
```

---

_Merged by @ibraheemdev on 2025-08-22 21:03_

---

_Closed by @ibraheemdev on 2025-08-22 21:03_

---

_Branch deleted on 2025-08-22 21:03_

---
