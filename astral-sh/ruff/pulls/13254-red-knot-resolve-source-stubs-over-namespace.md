```yaml
number: 13254
title: "[red-knot] resolve source/stubs over namespace packages"
type: pull_request
state: merged
author: Slyces
labels:
  - great writeup
  - ty
assignees: []
merged: true
base: main
head: fix/#12278-resolve-single-file-first
created_at: 2024-09-05T15:58:08Z
updated_at: 2024-09-06T11:14:26Z
url: https://github.com/astral-sh/ruff/pull/13254
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] resolve source/stubs over namespace packages

---

_Pull request opened by @Slyces on 2024-09-05 15:58_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

This PR tries to fix #12278. That issue doesn't have the tag `help-wanted`, so I'm not sure if I can contribute a fix

## Summary



### Expected Behaviour

In python, a `module` can actually be 3 things:
- Regular file
- Package (directory with `__init__.py`)
- Namespace package (directory without `__init__.py`)

In the import logic, when resolving `foo.bar.baz`, we should have the following priority:
```
└─ foo
   └─ bar
      ├─ baz           <-- regular package over module
      │  └─ __init__.py
      └─ baz.py
```
**works**
```
└─ foo
   └─ bar
      ├─ baz.py        <-- module over namespace package
      └─ baz           <-- namespace package invisible (because of module)
         └─ ...
```
**doesn't work**

### Current Implementation

Currently, the code properly imports regular packages over modules, but when a namespace package is here, it makes a module with the same name invisible, which is the opposite of what we want.

We want to guarantee 3 things:
- Regular package has priority over module (covered in existing test)
- Module has priority over namespace package (new test)
- A namespace package is made invisible by a module of the same name (new test)

### Fixes

We have 2 core problems in our logic:
- We properly resolve a namespace package when a module of the same name exists
- When a package (root, regular, namespace) is located and we try to resolve the last component `foo`
  - If `foo` is a directory, we consider it's a regular package
    - If the `foo` directory was a namespace package, it fails and we don't test `foo` as a module
  - If `foo` is not a directory, we try to resolve it as a module

This is fixed by:
- Adding a check in package resolution to skip shadowed namespace packages
- Changing the way we resolve modules to not rely on `foo` being a directory
  - We directly check `foo/__init__`
  - If that doesn't work, we fallback to `foo`
  
## Test Plan

The issue #12278 introduces a unit test that should pass after the issue is fixed.
This test is included in the PR.

---

_Review requested from @carljm by @Slyces on 2024-09-05 15:58_

---

_Review requested from @MichaReiser by @Slyces on 2024-09-05 15:58_

---

_Review requested from @AlexWaygood by @Slyces on 2024-09-05 15:58_

---

_Label `red-knot` added by @AlexWaygood on 2024-09-05 16:00_

---

_Label `great writeup` added by @MichaReiser on 2024-09-05 16:01_

---

_Comment by @carljm on 2024-09-05 16:02_

> That issue doesn't have the tag `help-wanted`, so I'm not sure if I can contribute a fix

There's no problem working on an issue that doesn't have the `help-wanted` tag, it just slightly increases the risk that there are unexplored complexities or we aren't sure how we want to address it yet :) It might be a good idea to comment on the issue before you start work, to reduce the chance of wasted work because two people work on an issue at once, or because the issue is stale, or whatever.

---

_Comment by @AlexWaygood on 2024-09-05 16:03_

And in this particular case, the contribution is very welcome!

---

_Comment by @github-actions[bot] on 2024-09-05 16:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @carljm on 2024-09-05 20:12_

Going to leave review on this one to Alex and Micha, who are more familiar with the module resolver implementation.

---

_Review request for @carljm removed by @carljm on 2024-09-05 20:12_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/path.rs`:72 on 2024-09-06 06:32_

It seems to me that we can push `kind` out of this function. As far as I can tell, the kind is always `Module` for line 573 and `Package` for line 578

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/path.rs`:68 on 2024-09-06 06:42_

Nit: I'm inclined to move this method out of `path` because `path` currently has no *resolution* mehtods. It only has methods to probe the structure. 

I do find the word *pure* unclear in this context. I think I know what you wanted to express with it. Maybe `resolve_file_module`? 

---

_@MichaReiser approved on 2024-09-06 06:42_

---

_@MichaReiser reviewed on 2024-09-06 06:43_

Overall looks great. Thank you. I've a few small nit comments and I'd like Alex to take a look as well

---

_@Slyces reviewed on 2024-09-06 07:20_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/module_resolver/path.rs`:68 on 2024-09-06 07:20_

The intention was to differentiate from `packages` which are also technically modules.
But I also think `resolve_file_module` is better (and achieves the same thing), thanks for the suggestion!

---

_@Slyces reviewed on 2024-09-06 09:10_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/module_resolver/path.rs`:68 on 2024-09-06 09:10_

Hi @MichaReiser, I've applied your comments:
- I keep a method is_file_module in the ModulePath to use in the resolve_package method
  (I think this fits the pattern of available methods)
- I moved the method resolve_file_module to resolve.rs

---

_Renamed from "[red-knot] resolve source/stubs over namespace packages, fixes #12278" to "[red-knot] resolve source/stubs over namespace packages" by @MichaReiser on 2024-09-06 09:14_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:645 on 2024-09-06 09:16_

I would remove `is_file_module` because `is_file_module` and `resolve_file_module` are now the same code except for the return type
```suggestion
            && resolve_file_mdoule(package_path, resolver_state).is_none()
```

---

_@MichaReiser reviewed on 2024-09-06 09:17_

---

_@AlexWaygood reviewed on 2024-09-06 09:22_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/path.rs`:69 on 2024-09-06 09:22_

I think I would call this method something like `with_any_python_extension()`, and have it return `Option<File>`. Callers of the method from `resolver.rs` that just need to know whether the path exists on disk with a Python extension can then just do `path.with_any_python_extensions(resolver).is_some()`, and we'll be able to remove the `resolve_file_module()` function from `resolver.rs` (because this will then do the same thing as that function):

```suggestion
    /// If `self` exists on disk with either a `.pyi` or `.py` extension,
    /// return the [`File`] corresponding to that path.
    ///
    /// `.pyi` files take priority, as they always have priority when
    /// resolving modules.
    #[must_use]
    pub(super) fn with_any_python_extension(&self, resolver: &ResolverContext) -> Option<File> {
        self.with_pyi_extension().to_file(resolver).or_else(|| {
            self
                .with_py_extension()
                .and_then(|path| path.to_file(resolver))
            })
    }
```

---

_@AlexWaygood reviewed on 2024-09-06 09:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/path.rs`:69 on 2024-09-06 09:23_

oh, I think you addressed this moments before I commented :-)

---

_@Slyces reviewed on 2024-09-06 09:25_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/module_resolver/path.rs`:69 on 2024-09-06 09:25_

So I think the method exists, right now it's named `is_file_module`, but I can agree on any name :)
It's also been moved to `resolve.rs` under MichaReiser's comments that it didn't really match existing methods in `ModulePath`

---

_@AlexWaygood reviewed on 2024-09-06 09:26_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/path.rs`:69 on 2024-09-06 09:26_

Yup, I agree that it doesn't really belong in `path.rs`, `resolver.rs` is better!

---

_@Slyces reviewed on 2024-09-06 09:28_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/module_resolver/path.rs`:69 on 2024-09-06 09:28_

I'll add the docstring though, I think it helps :)

---

_@AlexWaygood reviewed on 2024-09-06 09:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:584 on 2024-09-06 09:35_

This logic can be simplified slightly if we make the `ModulePath::join()` function in `path.rs` public (it already exists, but it's currently a test-only private function). Something like this should work (the diff is relative to your PR branch):

```diff
diff --git a/crates/red_knot_python_semantic/src/module_resolver/path.rs b/crates/red_knot_python_semantic/src/module_resolver/path.rs
index a49d1dc20..76c00b1c2 100644
--- a/crates/red_knot_python_semantic/src/module_resolver/path.rs
+++ b/crates/red_knot_python_semantic/src/module_resolver/path.rs
@@ -59,6 +59,13 @@ impl ModulePath {
         self.relative_path.push(component);
     }
 
+    #[must_use]
+    pub(crate) fn join(&self, component: &str) -> ModulePath {
+        let mut result = self.clone();
+        result.push(component);
+        result
+    }
+
     #[must_use]
     pub(super) fn is_directory(&self, resolver: &ResolverContext) -> bool {
         let ModulePath {
@@ -634,15 +641,6 @@ mod tests {
 
     use super::*;
 
-    impl ModulePath {
-        #[must_use]
-        fn join(&self, component: &str) -> ModulePath {
-            let mut result = self.clone();
-            result.push(component);
-            result
-        }
-    }
-
     impl SearchPath {
         fn join(&self, component: &str) -> ModulePath {
             self.to_module_path().join(component)
diff --git a/crates/red_knot_python_semantic/src/module_resolver/resolver.rs b/crates/red_knot_python_semantic/src/module_resolver/resolver.rs
index 50aaf9dbd..294ad477d 100644
--- a/crates/red_knot_python_semantic/src/module_resolver/resolver.rs
+++ b/crates/red_knot_python_semantic/src/module_resolver/resolver.rs
@@ -569,18 +569,13 @@ fn resolve_name(db: &dyn Db, name: &ModuleName) -> Option<(SearchPath, File, Mod
 
                 package_path.push(module_name);
 
-                // Try to resolve a stub/module with the current name
-                let file_module = resolve_file_module(&package_path, &resolver_state);
-
-                // Then check if a regular package exists with the same name (it takes
-                // precedence)
-                package_path.push("__init__");
-                if let Some(regular_package) = resolve_file_module(&package_path, &resolver_state) {
+                // Regular packages take first priority:
+                if let Some(regular_package) = resolve_file_module(&package_path.join("__init__"), &resolver_state) {
                     return Some((search_path.clone(), regular_package, ModuleKind::Package));
                 }
 
-                // If we found a module (but no package), return it
-                if let Some(module) = file_module {
+                // If there's a single-file module (but no package), return it
+                if let Some(module) = resolve_file_module(&package_path, &resolver_state) {
                     return Some((search_path.clone(), module, ModuleKind::Module));
                 }
```

---

_@MichaReiser reviewed on 2024-09-06 09:44_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:584 on 2024-09-06 09:44_

The reason for using `push` is because it avoids allocating a new path. 

---

_@AlexWaygood reviewed on 2024-09-06 09:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:584 on 2024-09-06 09:50_

My main point is that if the path resolves to a regular package, we're doing an unnecessary `resolve_file_module()` call. I doubt it matters much in terms of performance but I think it could be written slightly more clearly. If we want to avoid allocating, we could add a `.pop()` method:

```diff
--- a/crates/red_knot_python_semantic/src/module_resolver/path.rs
+++ b/crates/red_knot_python_semantic/src/module_resolver/path.rs
@@ -59,6 +59,10 @@ impl ModulePath {
         self.relative_path.push(component);
     }
 
+    pub(crate) fn pop(&mut self) -> bool {
+        self.relative_path.pop()
+    }
+
     #[must_use]
     pub(super) fn is_directory(&self, resolver: &ResolverContext) -> bool {
         let ModulePath {
diff --git a/crates/red_knot_python_semantic/src/module_resolver/resolver.rs b/crates/red_knot_python_semantic/src/module_resolver/resolver.rs
index 68f08118e..c563d0b80 100644
--- a/crates/red_knot_python_semantic/src/module_resolver/resolver.rs
+++ b/crates/red_knot_python_semantic/src/module_resolver/resolver.rs
@@ -569,9 +569,6 @@ fn resolve_name(db: &dyn Db, name: &ModuleName) -> Option<(SearchPath, File, Mod
 
                 package_path.push(module_name);
 
-                // Try to resolve a stub/module with the current name
-                let file_module = resolve_file_module(&package_path, &resolver_state);
-
                 // Then check if a regular package exists with the same name (it takes
                 // precedence)
                 package_path.push("__init__");
@@ -579,8 +576,9 @@ fn resolve_name(db: &dyn Db, name: &ModuleName) -> Option<(SearchPath, File, Mod
                     return Some((search_path.clone(), regular_package, ModuleKind::Package));
                 }
 
-                // If we found a module (but no package), return it
-                if let Some(module) = file_module {
+                // If we can find a module (but no package), return it
+                package_path.pop();
+                if let Some(module) = resolve_file_module(&package_path, &resolver_state) {
                     return Some((search_path.clone(), module, ModuleKind::Module));
                 }
```

---

_@Slyces reviewed on 2024-09-06 10:10_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:584 on 2024-09-06 10:10_

I think I find either options (`join` or `pop`) clearer than the existing implementation

---

_@AlexWaygood approved on 2024-09-06 10:40_

Looks great. Thanks, this was an excellent contribution @Slyces! :D

---

_Merged by @AlexWaygood on 2024-09-06 11:14_

---

_Closed by @AlexWaygood on 2024-09-06 11:14_

---
