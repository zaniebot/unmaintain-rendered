```yaml
number: 20909
title: "[ty] Add version hint for failed stdlib accesses"
type: pull_request
state: merged
author: Gankra
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: gankra/older-python
created_at: 2025-10-16T03:00:47Z
updated_at: 2025-10-16T14:07:34Z
url: https://github.com/astral-sh/ruff/pull/20909
synced_at: 2026-01-12T15:57:12Z
```

# [ty] Add version hint for failed stdlib accesses

---

_@Gankra_

This is the ultra-minimal implementation of

* https://github.com/astral-sh/ty/issues/296

that was previously discussed as a good starting point. In particular we don't actually bother trying to figure out the exact python versions, but we still mention "hey btw for No Reason At All... you're on python 3.10" when you try to access something that has a definition rooted in the stdlib that we believe exists sometimes.


---

_Review requested from @carljm by @Gankra on 2025-10-16 03:00_

---

_Review requested from @AlexWaygood by @Gankra on 2025-10-16 03:00_

---

_Review requested from @sharkdp by @Gankra on 2025-10-16 03:00_

---

_Review requested from @dcreager by @Gankra on 2025-10-16 03:00_

---

_Label `ty` added by @Gankra on 2025-10-16 03:00_

---

_Label `diagnostics` added by @Gankra on 2025-10-16 03:00_

---

_Comment by @github-actions[bot] on 2025-10-16 03:02_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-16 03:04_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- Found 51 diagnostics
+ Found 52 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Review requested from @MichaReiser by @Gankra on 2025-10-16 03:09_

---

_@Gankra reviewed on 2025-10-16 03:10_

---

_Review comment by @Gankra on `crates/ty/tests/cli/python_environment.rs`:980 on 2025-10-16 03:10_

Oh boy these are really verbose when it's not "from the CLI"

---

_@Gankra reviewed on 2025-10-16 03:11_

---

_Review comment by @Gankra on `crates/ty_python_semantic/resources/mdtest/snapshots/super.md_-_Super_-_Basic_Usage_-_Explicit_Super_Objec…_(b753048091f275c0).snap`:132 on 2025-10-16 03:11_

These firing on `super` is so sad, I'm not sure if there's a good General rule that would exclude this and similar types.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/diagnostic.rs`:3149 on 2025-10-16 06:43_

```suggestion
/// stsnfstf library and `foo.bar` *does* exist as an attribute on *other*
```

The what library? `stsnfstf` 

---

_Review comment by @MichaReiser on `crates/ty/tests/cli/python_environment.rs`:980 on 2025-10-16 07:22_

Yeah. Rustc hides very verbose diagnostic details unless ran with `-v`. I added the infrastructure for it in https://github.com/astral-sh/ruff/pull/20912 but I also think it's sort of fine here since those diagnostics should be rare

---

_@MichaReiser approved on 2025-10-16 07:22_

---

_Comment by @sharkdp on 2025-10-16 07:24_

To be honest, I'm not sure if this initial step is a strict improvement. Sure, it seems helpful in the rare case where you are accessing an attribute that is not available on the Python version that you have configured, *but is available under the exact name on a higher version*.

But in the vast majority of `unresolved-attribute` cases (e.g. a misspelled attribute name, or an incomplete attribute name, because I'm still typing it), it just adds additional noise?

```py
"test.exe".trimsuffix(".exe")  # no, it's called "replacesuffix"
```

```
error[unresolved-attribute]: Type `Literal["test.exe"]` has no attribute `trimsuffix`
 --> /home/shark/playground/test.py:4:1
  |
4 | "test.exe".trimsuffix(".exe")
  | ^^^^^^^^^^^^^^^^^^^^^
  |
info: Python 3.14 was assumed when accessing `trimsuffix` because it was specified on the command line
info: rule `unresolved-attribute` is enabled by default
```

Note that the diagnostic change here does not show up in the ecosystem diff because `info` hints are not part of the concise message, but we have thousands of `unresolved-attribute` diagnostics [across the ecosystem](https://ecosystem.ty.dev/), most of which will be more verbose with this change.

---

_@AlexWaygood reviewed on 2025-10-16 10:52_

Yeah, I agree with @sharkdp that in its current state this could just add a lot of noise and not be that helpful :-(

But I think it's pretty easy to make a few adjustments to this so that it's less noisy, but still pretty useful. Something like this, where we do actually check that the exact symbol exists in the symbol table _somewhere_ (the definition just isn't visible on this Python version), could work pretty well:

```diff
diff --git a/crates/ty_python_semantic/src/semantic_index/place.rs b/crates/ty_python_semantic/src/semantic_index/place.rs
index 38e3d92816..75d3eaac65 100644
--- a/crates/ty_python_semantic/src/semantic_index/place.rs
+++ b/crates/ty_python_semantic/src/semantic_index/place.rs
@@ -181,7 +181,6 @@ impl PlaceTable {
     }
 
     /// Looks up a symbol by its name and returns a reference to it, if it exists.
-    #[cfg(test)]
     pub(crate) fn symbol_by_name(&self, name: &str) -> Option<&Symbol> {
         self.symbols.symbol_id(name).map(|id| self.symbol(id))
     }
diff --git a/crates/ty_python_semantic/src/types/diagnostic.rs b/crates/ty_python_semantic/src/types/diagnostic.rs
index f07d934dad..cb6bf8c4e7 100644
--- a/crates/ty_python_semantic/src/types/diagnostic.rs
+++ b/crates/ty_python_semantic/src/types/diagnostic.rs
@@ -9,6 +9,7 @@ use crate::lint::{Level, LintRegistryBuilder, LintStatus};
 use crate::module_resolver::file_to_module;
 use crate::semantic_index::definition::{Definition, DefinitionKind};
 use crate::semantic_index::place::{PlaceTable, ScopedPlaceId};
+use crate::semantic_index::{global_scope, place_table};
 use crate::suppression::FileSuppressionId;
 use crate::types::call::CallError;
 use crate::types::class::{DisjointBase, DisjointBaseKind, Field};
@@ -3155,6 +3156,16 @@ pub(super) fn hint_if_stdlib_attribute_exists_on_other_versions(
     value_type: &Type,
     attr: &Identifier,
 ) {
+    let Type::ModuleLiteral(module) = *value_type else {
+        return;
+    };
+    let Some(file) = module.module(db).file(db) else {
+        return;
+    };
+    let symbol_table = place_table(db, global_scope(db, file));
+    if symbol_table.symbol_by_name(attr).is_none() {
+        return;
+    }
     let Some(definition) = value_type.definition(db) else {
         return;
     };
```

This edit would mean that the suggestions would only fire on `unresolved-attribute` errors involving module-literal types. But I think probably most "oops, I tried to access this attribute that only exists on a new Python version" errors involve module-literal objects. So I think even this limited hint would help a lot of users and be a great first step.

---

_Comment by @Gankra on 2025-10-16 13:22_

Heh, I considered that implementation originally and was like "wait I can do better, and modules are their own definitions so this handles those too!"

Having seen the results, I agree it's a safer minimum.

---

_Comment by @Gankra on 2025-10-16 13:46_

Pushed up the new implementation.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:3168 on 2025-10-16 13:52_

```suggestion
    // Version-dependent definitions are much more common in the stdlib
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:3175 on 2025-10-16 13:53_

it could also be in the symbol table for the global scope, but without a visible definition, if it's a platform-specific definition (the definition only exists in amn `if sys.platform == "linux"` branch, but their configured platform is `"win32"`). We could consider also highlighting the user's configured platform as well as their configured Python version. Doesn't need to be done in this PR, though

---

_@AlexWaygood approved on 2025-10-16 13:54_

Thank you, this is great!

---

_Merged by @Gankra on 2025-10-16 14:07_

---

_Closed by @Gankra on 2025-10-16 14:07_

---

_Branch deleted on 2025-10-16 14:07_

---
