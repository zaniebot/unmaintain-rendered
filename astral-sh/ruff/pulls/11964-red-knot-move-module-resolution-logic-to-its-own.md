```yaml
number: 11964
title: "[red-knot] Move module-resolution logic to its own crate"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: module-resolver-crate
created_at: 2024-06-21T12:25:28Z
updated_at: 2024-06-21T13:27:15Z
url: https://github.com/astral-sh/ruff/pull/11964
synced_at: 2026-01-12T15:55:39Z
```

# [red-knot] Move module-resolution logic to its own crate

---

_@AlexWaygood_

## Summary

This PR moves module-resolution logic to its own crate. It attempts to make the minimal possible changes to do that, so that future PRs can build on top of it. As a result, there's a few things that this PR makes `pub` that probably shouldn't be `pub` in the long-term.

The PR... nearly compiles. I may need @MichaReiser's help for the remaining compile errors.

---

_Label `red-knot` added by @AlexWaygood on 2024-06-21 12:25_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-21 12:29_

---

_Comment by @MichaReiser on 2024-06-21 12:51_

I took a quick look. 

* You have to remove the resolver "ingredients" from the semantic `Jar`, they're already part of the resolver `Jar`. 
* You have to call `db.upcast()` when calling resolver queries from the semantic crate

```patch
Subject: [PATCH] Don't lazily infer imports because it adds an AST dependency to the module looking up the definition
---
Index: crates/red_knot_python_semantic/src/types/infer.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/src/types/infer.rs
--- a/crates/red_knot_python_semantic/src/types/infer.rs	(revision 63049cd8fc5a8f9d23fdac415bfd4b8622fff887)
+++ b/crates/red_knot_python_semantic/src/types/infer.rs	(date 1718974225963)
@@ -358,7 +358,7 @@
             } = alias;
 
             let module_name = ModuleName::new(&name.id);
-            let module = module_name.and_then(|name| resolve_module(self.db, name));
+            let module = module_name.and_then(|name| resolve_module(self.db.upcast(), name));
             let module_ty = module
                 .map(|module| self.typing_context().module_ty(module.file()))
                 .unwrap_or(Type::Unknown);
@@ -384,7 +384,8 @@
         let import_id = import.scope_ast_id(self.db, self.file_id, self.file_scope_id);
         let module_name = ModuleName::new(module.as_deref().expect("Support relative imports"));
 
-        let module = module_name.and_then(|module_name| resolve_module(self.db, module_name));
+        let module =
+            module_name.and_then(|module_name| resolve_module(self.db.upcast(), module_name));
         let module_ty = module
             .map(|module| self.typing_context().module_ty(module.file()))
             .unwrap_or(Type::Unknown);
Index: crates/red_knot_python_semantic/src/db.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/red_knot_python_semantic/src/db.rs b/crates/red_knot_python_semantic/src/db.rs
--- a/crates/red_knot_python_semantic/src/db.rs	(revision 63049cd8fc5a8f9d23fdac415bfd4b8622fff887)
+++ b/crates/red_knot_python_semantic/src/db.rs	(date 1718974206963)
@@ -14,13 +14,9 @@
 
 #[salsa::jar(db=Db)]
 pub struct Jar(
-    ModuleNameIngredient<'_>,
-    ModuleResolverSearchPaths,
     ScopeId<'_>,
     PublicSymbolId<'_>,
     symbol_table,
-    resolve_module_query,
-    file_to_module,
     scopes_map,
     root_scope,
     semantic_index,

```


---

_@MichaReiser reviewed on 2024-06-21 13:10_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:186 on 2024-06-21 13:10_

It should be possible to keep the struct here, and `internal` pub(crate)`

---

_@MichaReiser approved on 2024-06-21 13:11_

---

_@AlexWaygood reviewed on 2024-06-21 13:12_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:186 on 2024-06-21 13:12_

Thanks! I was already working on this before you commented ;)

---

_Marked ready for review by @AlexWaygood on 2024-06-21 13:13_

---

_Review requested from @carljm by @AlexWaygood on 2024-06-21 13:13_

---

_Merged by @AlexWaygood on 2024-06-21 13:25_

---

_Closed by @AlexWaygood on 2024-06-21 13:25_

---

_Branch deleted on 2024-06-21 13:27_

---
