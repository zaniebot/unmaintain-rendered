```yaml
number: 12224
title: "[red-knot] Use vendored typeshed stubs for stdlib module resolution"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: module-resolver-vendored-paths-2
created_at: 2024-07-07T22:01:49Z
updated_at: 2024-07-09T09:31:05Z
url: https://github.com/astral-sh/ruff/pull/12224
synced_at: 2026-01-12T15:55:40Z
```

# [red-knot] Use vendored typeshed stubs for stdlib module resolution

---

_@AlexWaygood_

## Summary

If the user does not pass a `--custom-typeshed-dir` flag via the CLI or via a configuration flag, fall back to using the `VendoredFileSystem` to resolve standard-library modules in the module resolver. Work towards #11653.

(This PR is stacked on top of #12215)

## Test Plan

I added a test to `resolver.rs` that demonstrates that vendored files now work


---

_Label `red-knot` added by @AlexWaygood on 2024-07-07 22:01_

---

_Review requested from @carljm by @AlexWaygood on 2024-07-07 22:01_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-07-07 22:01_

---

_Comment by @github-actions[bot] on 2024-07-07 22:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored/path.rs`:45 on 2024-07-08 08:22_


```suggestion
    pub fn join(&self, other: impl AsRef<VendoredPath>) -> VendoredPathBuf {
```

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored/path.rs`:66 on 2024-07-08 08:22_


```suggestion
        prefix: impl AsRef<VendoredPath>,
```

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored/path.rs`:91 on 2024-07-08 08:23_


```suggestion
    pub fn push(&mut self, component: impl AsRef<VendoredPath>) {
```

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/db.rs`:135 on 2024-07-08 08:26_

Nit: I would align the `with_vendored_stubs` and `use_mocked_typeshed`. Now the variable and method name are exactly the opposite, making it hard to see that the two are related. 

I'm prefering `use_vendored_stubs` 

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/db.rs`:193 on 2024-07-08 08:30_

Maybe we could have two versions of it. The basic functionality we need is to have a way to specify a custom typeshed directory. 

The second part is to set up `fs` with the typeshed stubs. I think I would change the API (and field) to

```rust
pub(crate) fn use_custom_typeshed_directory(mut self, directory: &SystemPath) -> Self {
    self.custom_typeshed_directory = Some(directory.to_path_buf());
    self
}

pub(crate) fn use_mocked_typeshed(mut self) -> Self {
    let fs = self.db.system().memory_file_system();

    // ... write files

    Self::use_custom_typeshed_directory(directory)
}
```

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/typeshed/versions.rs`:89 on 2024-07-08 08:31_

Doesn't that mean that we now include the file twice? What's the reason why you decided not to read the file from the vendored file system

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/typeshed/versions.rs`:45 on 2024-07-08 08:32_

Nit: The `db` argument should always be the first argument for consistency

---

_Review comment by @MichaReiser on `crates/ruff_db/src/files/path.rs`:186 on 2024-07-08 09:11_

I'm very hesitant of adding a `Ref` version. It's a lot of code to maintain because we have to duplicate very single method to keep a consistent API (e.g. `parent` is currently missing from `FilePath`). I wouldn't mind doing it we could implement `AsRef` but that's, unfortunately not possible. 

I also think that a `PathRef` version wouldn't be necessary once we address:

```
/// Internal abstractions for differentiating between different kinds of search paths.
///
/// TODO(Alex): Should we use different types for absolute vs relative paths?
/// <https://github.com/astral-sh/ruff/pull/12141#discussion_r1667010245>
```

Because we could then have an owned `Path` for the module search path roots and use `Utf8Path` and `Utf8PathBuf` for the relative module paths. That means, we get away by just using `FilePath`. 

I'm okay deferring the above refactor but I want to avoid that we introduce more extra code just to work around it for not. 

To unblock us now, my recommendation is to have a resolver local `StandardLibraryPathRef` enum type. It does require moving some code from `ModuleResolutionPathRef` to `ModuleResolutionPathBuf` but I think that's fine. See the following patch.

<details>
<summary>Patch</summary>

```patch
Subject: [PATCH] Handle cancellation (or at least, try)
---
Index: crates/red_knot_module_resolver/src/path.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/red_knot_module_resolver/src/path.rs b/crates/red_knot_module_resolver/src/path.rs
--- a/crates/red_knot_module_resolver/src/path.rs	(revision 129b3b9ac3148b2fe1a93a7d83ef775f875dc26d)
+++ b/crates/red_knot_module_resolver/src/path.rs	(date 1720429732897)
@@ -1,4 +1,8 @@
-use ruff_db::files::{system_path_to_file, File, FilePath, FilePathRef};
+use crate::module_name::ModuleName;
+use crate::state::ResolverState;
+use crate::typeshed::TypeshedVersionsQueryResult;
+use camino::Utf8Components;
+use ruff_db::files::{system_path_to_file, File, FilePath};
 use ruff_db::system::{SystemPath, SystemPathBuf};
 use ruff_db::vendored::{VendoredPath, VendoredPathBuf};
 /// Internal abstractions for differentiating between different kinds of search paths.
@@ -7,10 +11,6 @@
 /// <https://github.com/astral-sh/ruff/pull/12141#discussion_r1667010245>
 use std::fmt;
 
-use crate::module_name::ModuleName;
-use crate::state::ResolverState;
-use crate::typeshed::TypeshedVersionsQueryResult;
-
 /// Enumeration of the different kinds of search paths type checkers are expected to support.
 ///
 /// N.B. Although we don't implement `Ord` for this enum, they are ordered in terms of the
@@ -150,12 +150,12 @@
 
     #[must_use]
     pub(crate) fn is_regular_package(&self, search_path: &Self, resolver: &ResolverState) -> bool {
-        ModuleResolutionPathRef::from(self).is_regular_package(search_path, resolver)
+        self.0.is_regular_package(&search_path.0, resolver)
     }
 
     #[must_use]
     pub(crate) fn is_directory(&self, search_path: &Self, resolver: &ResolverState) -> bool {
-        ModuleResolutionPathRef::from(self).is_directory(search_path, resolver)
+        self.0.is_directory(&search_path.0, resolver)
     }
 
     #[must_use]
@@ -170,83 +170,33 @@
 
     #[must_use]
     pub(crate) fn relativize_path<'a>(
-        &'a self,
-        absolute_path: &FilePathRef<'a>,
+        &self,
+        absolute_path: &'a FilePath,
     ) -> Option<ModuleResolutionPathRef<'a>> {
-        ModuleResolutionPathRef::from(self).relativize_path(absolute_path)
+        self.0
+            .relativize_path(absolute_path)
+            .map(ModuleResolutionPathRef)
     }
 
     /// Returns `None` if the path doesn't exist, isn't accessible, or if the path points to a directory.
     pub(crate) fn to_file(&self, search_path: &Self, resolver: &ResolverState) -> Option<File> {
-        ModuleResolutionPathRef::from(self).to_file(search_path, resolver)
-    }
-}
-
-impl fmt::Debug for ModuleResolutionPathBuf {
-    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
-        match &self.0 {
-            ModuleResolutionPathBufInner::Extra(path) => f
-                .debug_tuple("ModuleResolutionPathBuf::Extra")
-                .field(path)
-                .finish(),
-            ModuleResolutionPathBufInner::FirstParty(path) => f
-                .debug_tuple("ModuleResolutionPathBuf::FirstParty")
-                .field(path)
-                .finish(),
-            ModuleResolutionPathBufInner::SitePackages(path) => f
-                .debug_tuple("ModuleResolutionPathBuf::SitePackages")
-                .field(path)
-                .finish(),
-            ModuleResolutionPathBufInner::StandardLibrary(path) => f
-                .debug_tuple("ModuleResolutionPathBuf::StandardLibrary")
-                .field(path)
-                .finish(),
-        }
+        self.0.to_file(&search_path.0, resolver)
     }
 }
 
-#[derive(Debug, PartialEq, Eq, Hash, Clone, Copy)]
-enum ModuleResolutionPathRefInner<'a> {
-    Extra(&'a SystemPath),
-    FirstParty(&'a SystemPath),
-    StandardLibrary(FilePathRef<'a>),
-    SitePackages(&'a SystemPath),
-}
-
-impl<'a> ModuleResolutionPathRefInner<'a> {
+impl ModuleResolutionPathBufInner {
     #[must_use]
-    fn query_stdlib_version<'db>(
-        module_path: &FilePathRef<'a>,
-        stdlib_search_path: Self,
-        stdlib_root: &FilePathRef<'a>,
-        resolver_state: &ResolverState<'db>,
-    ) -> TypeshedVersionsQueryResult {
-        let Some(module_name) = stdlib_search_path
-            .relativize_path(module_path)
-            .and_then(Self::to_module_name)
-        else {
-            return TypeshedVersionsQueryResult::DoesNotExist;
-        };
-        let ResolverState {
-            db,
-            typeshed_versions,
-            target_version,
-        } = resolver_state;
-        typeshed_versions.query_module(&module_name, *db, stdlib_root, *target_version)
-    }
-
-    #[must_use]
-    fn is_directory(&self, search_path: Self, resolver: &ResolverState) -> bool {
-        match (self, search_path) {
+    fn is_directory(&self, search_path: &Self, resolver: &ResolverState) -> bool {
+        match (self, &search_path) {
             (Self::Extra(path), Self::Extra(_)) => resolver.system().is_directory(path),
             (Self::FirstParty(path), Self::FirstParty(_)) => resolver.system().is_directory(path),
             (Self::SitePackages(path), Self::SitePackages(_)) => resolver.system().is_directory(path),
             (Self::StandardLibrary(path), Self::StandardLibrary(stdlib_root)) => {
-                match Self::query_stdlib_version(path, search_path, &stdlib_root, resolver) {
+                match Self::query_stdlib_version(path, search_path, stdlib_root, resolver) {
                     TypeshedVersionsQueryResult::DoesNotExist => false,
                     TypeshedVersionsQueryResult::Exists | TypeshedVersionsQueryResult::MaybeExists => match path {
-                        FilePathRef::System(path) => resolver.system().is_directory(path),
-                        FilePathRef::Vendored(path) => resolver.vendored().is_directory(path)
+                        FilePath::System(path) => resolver.system().is_directory(path),
+                        FilePath::Vendored(path) => resolver.vendored().is_directory(path)
                     }
                 }
             }
@@ -257,7 +207,7 @@
     }
 
     #[must_use]
-    fn is_regular_package(&self, search_path: Self, resolver: &ResolverState) -> bool {
+    fn is_regular_package(&self, search_path: &Self, resolver: &ResolverState) -> bool {
         fn is_non_stdlib_pkg(state: &ResolverState, path: &SystemPath) -> bool {
             let file_system = state.system();
             file_system.exists(&path.join("__init__.py"))
@@ -272,11 +222,11 @@
             // (1) Account for VERSIONS
             // (2) Only test for `__init__.pyi`, not `__init__.py`
             (Self::StandardLibrary(path), Self::StandardLibrary(stdlib_root)) => {
-                match Self::query_stdlib_version( path, search_path, &stdlib_root, resolver) {
+                match Self::query_stdlib_version(path, search_path, &stdlib_root, resolver) {
                     TypeshedVersionsQueryResult::DoesNotExist => false,
                     TypeshedVersionsQueryResult::Exists | TypeshedVersionsQueryResult::MaybeExists => match path {
-                        FilePathRef::System(path) => resolver.db.system().exists(&path.join("__init__.pyi")),
-                        FilePathRef::Vendored(path) => resolver.db.vendored().exists(path.join("__init__.pyi")),
+                        FilePath::System(path) => resolver.db.system().exists(&path.join("__init__.pyi")),
+                        FilePath::Vendored(path) => resolver.db.vendored().exists(path.join("__init__.pyi")),
                     },
                 }
             }
@@ -286,7 +236,7 @@
         }
     }
 
-    fn to_file(self, search_path: Self, resolver: &ResolverState) -> Option<File> {
+    fn to_file(&self, search_path: &Self, resolver: &ResolverState) -> Option<File> {
         match (self, search_path) {
             (Self::Extra(path), Self::Extra(_)) => system_path_to_file(resolver.db.upcast(), path),
             (Self::FirstParty(path), Self::FirstParty(_)) => system_path_to_file(resolver.db.upcast(), path),
@@ -306,35 +256,149 @@
     }
 
     #[must_use]
-    fn to_module_name(self) -> Option<ModuleName> {
+    fn query_stdlib_version<'db>(
+        module_path: &FilePath,
+        stdlib_search_path: &Self,
+        stdlib_root: &FilePath,
+        resolver_state: &ResolverState<'db>,
+    ) -> TypeshedVersionsQueryResult {
+        let Some(module_name) = stdlib_search_path
+            .relativize_path(module_path)
+            .and_then(|path| path.to_module_name())
+        else {
+            return TypeshedVersionsQueryResult::DoesNotExist;
+        };
+        let ResolverState {
+            db,
+            typeshed_versions,
+            target_version,
+        } = resolver_state;
+        typeshed_versions.query_module(&module_name, *db, stdlib_root, *target_version)
+    }
+
+    #[must_use]
+    fn relativize_path<'a>(
+        &self,
+        absolute_path: &'a FilePath,
+    ) -> Option<ModuleResolutionPathRefInner<'a>> {
+        match (self, absolute_path) {
+            (Self::Extra(root), FilePath::System(absolute_path)) => {
+                absolute_path.strip_prefix(root).ok().and_then(|path| {
+                    path.extension()
+                        .map_or(true, |ext| matches!(ext, "py" | "pyi"))
+                        .then_some(ModuleResolutionPathRefInner::Extra(path))
+                })
+            }
+            (Self::FirstParty(root), FilePath::System(absolute_path)) => {
+                absolute_path.strip_prefix(root).ok().and_then(|path| {
+                    path.extension()
+                        .map_or(true, |ext| matches!(ext, "pyi" | "py"))
+                        .then_some(ModuleResolutionPathRefInner::FirstParty(path))
+                })
+            }
+            (Self::StandardLibrary(root), FilePath::System(absolute_path)) => match root {
+                FilePath::System(root) => absolute_path.strip_prefix(root).ok().and_then(|path| {
+                    path.extension().map_or(true, |ext| ext == "pyi").then_some(
+                        ModuleResolutionPathRefInner::StandardLibrary(StandardLibraryPath::System(
+                            path,
+                        )),
+                    )
+                }),
+                FilePath::Vendored(_) => None,
+            },
+            (Self::SitePackages(root), FilePath::System(absolute_path)) => {
+                absolute_path.strip_prefix(root).ok().and_then(|path| {
+                    path.extension()
+                        .map_or(true, |ext| matches!(ext, "pyi" | "py"))
+                        .then_some(ModuleResolutionPathRefInner::SitePackages(path))
+                })
+            }
+            (Self::Extra(_), FilePath::Vendored(_)) => None,
+            (Self::FirstParty(_), FilePath::Vendored(_)) => None,
+            (Self::StandardLibrary(root), FilePath::Vendored(absolute_path)) => match root {
+                FilePath::System(_) => None,
+                FilePath::Vendored(root) => {
+                    absolute_path.strip_prefix(&root).ok().and_then(|path| {
+                        path.extension().map_or(true, |ext| ext == "pyi").then_some(
+                            ModuleResolutionPathRefInner::StandardLibrary(
+                                StandardLibraryPath::Vendored(path),
+                            ),
+                        )
+                    })
+                }
+            },
+            (Self::SitePackages(_), FilePath::Vendored(_)) => None,
+        }
+    }
+}
+
+impl fmt::Debug for ModuleResolutionPathBuf {
+    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
+        match &self.0 {
+            ModuleResolutionPathBufInner::Extra(path) => f
+                .debug_tuple("ModuleResolutionPathBuf::Extra")
+                .field(path)
+                .finish(),
+            ModuleResolutionPathBufInner::FirstParty(path) => f
+                .debug_tuple("ModuleResolutionPathBuf::FirstParty")
+                .field(path)
+                .finish(),
+            ModuleResolutionPathBufInner::SitePackages(path) => f
+                .debug_tuple("ModuleResolutionPathBuf::SitePackages")
+                .field(path)
+                .finish(),
+            ModuleResolutionPathBufInner::StandardLibrary(path) => f
+                .debug_tuple("ModuleResolutionPathBuf::StandardLibrary")
+                .field(path)
+                .finish(),
+        }
+    }
+}
+
+#[derive(Debug, PartialEq, Eq, Hash, Clone, Copy)]
+enum ModuleResolutionPathRefInner<'a> {
+    Extra(&'a SystemPath),
+    FirstParty(&'a SystemPath),
+    StandardLibrary(StandardLibraryPath<'a>),
+    SitePackages(&'a SystemPath),
+}
+
+#[derive(Debug, PartialEq, Eq, Hash, Clone, Copy)]
+enum StandardLibraryPath<'a> {
+    System(&'a SystemPath),
+    Vendored(&'a VendoredPath),
+}
+
+impl<'a> StandardLibraryPath<'a> {
+    #[cfg(test)]
+    fn system(path: &'a str) -> Self {
+        Self::System(SystemPath::new(path))
+    }
+
+    fn components(&self) -> Utf8Components {
         match self {
-            Self::Extra(path) | Self::FirstParty(path) | Self::SitePackages(path) => {
-                let parent_components = path
-                    .parent()?
-                    .components()
-                    .map(|component| component.as_str());
-                if path.ends_with("__init__.py") || path.ends_with("__init__.pyi") {
-                    ModuleName::from_components(parent_components)
-                } else {
-                    ModuleName::from_components(parent_components.chain(path.file_stem()))
-                }
-            }
-            Self::StandardLibrary(path) => {
-                let parent = path.parent()?;
-                let parent_components = parent.components().map(|component| component.as_str());
-                let skip_final_part = match path {
-                    FilePathRef::System(path) => path.ends_with("__init__.pyi"),
-                    FilePathRef::Vendored(path) => path.ends_with("__init__.pyi"),
-                };
-                if skip_final_part {
-                    ModuleName::from_components(parent_components)
-                } else {
-                    ModuleName::from_components(parent_components.chain(path.file_stem()))
-                }
-            }
+            StandardLibraryPath::System(path) => path.components(),
+            StandardLibraryPath::Vendored(path) => path.components(),
+        }
+    }
+
+    fn file_stem(&self) -> Option<&str> {
+        match self {
+            StandardLibraryPath::System(path) => path.file_stem(),
+            StandardLibraryPath::Vendored(path) => path.file_stem(),
+        }
+    }
+
+    #[cfg(test)]
+    fn to_path_buf(self) -> FilePath {
+        match self {
+            StandardLibraryPath::System(path) => FilePath::System(path.to_path_buf()),
+            StandardLibraryPath::Vendored(path) => FilePath::Vendored(path.to_path_buf()),
         }
     }
+}
 
+impl<'a> ModuleResolutionPathRefInner<'a> {
     #[must_use]
     fn with_pyi_extension(&self) -> ModuleResolutionPathBufInner {
         match self {
@@ -342,12 +406,12 @@
             Self::FirstParty(path) => {
                 ModuleResolutionPathBufInner::FirstParty(path.with_extension("pyi"))
             }
-            Self::StandardLibrary(FilePathRef::System(path)) => {
+            Self::StandardLibrary(StandardLibraryPath::System(path)) => {
                 ModuleResolutionPathBufInner::StandardLibrary(FilePath::System(
                     path.with_extension("pyi"),
                 ))
             }
-            Self::StandardLibrary(FilePathRef::Vendored(path)) => {
+            Self::StandardLibrary(StandardLibraryPath::Vendored(path)) => {
                 ModuleResolutionPathBufInner::StandardLibrary(FilePath::Vendored(
                     path.with_pyi_extension(),
                 ))
@@ -375,52 +439,39 @@
     }
 
     #[must_use]
-    fn relativize_path(&self, absolute_path: &FilePathRef<'a>) -> Option<Self> {
-        match (self, absolute_path) {
-            (Self::Extra(root), FilePathRef::System(absolute_path)) => {
-                absolute_path.strip_prefix(root).ok().and_then(|path| {
-                    path.extension()
-                        .map_or(true, |ext| matches!(ext, "py" | "pyi"))
-                        .then_some(Self::Extra(path))
-                })
-            }
-            (Self::FirstParty(root), FilePathRef::System(absolute_path)) => {
-                absolute_path.strip_prefix(root).ok().and_then(|path| {
-                    path.extension()
-                        .map_or(true, |ext| matches!(ext, "pyi" | "py"))
-                        .then_some(Self::FirstParty(path))
-                })
+    fn to_module_name(&self) -> Option<ModuleName> {
+        match self {
+            Self::Extra(path) | Self::FirstParty(path) | Self::SitePackages(path) => {
+                let parent_components = path
+                    .parent()?
+                    .components()
+                    .map(|component| component.as_str());
+                if path.ends_with("__init__.py") || path.ends_with("__init__.pyi") {
+                    ModuleName::from_components(parent_components)
+                } else {
+                    ModuleName::from_components(parent_components.chain(path.file_stem()))
+                }
             }
-            (Self::StandardLibrary(root), FilePathRef::System(absolute_path)) => match root {
-                FilePathRef::System(root) => {
-                    absolute_path.strip_prefix(root).ok().and_then(|path| {
-                        path.extension()
-                            .map_or(true, |ext| ext == "pyi")
-                            .then_some(Self::StandardLibrary(FilePathRef::System(path)))
-                    })
-                }
-                FilePathRef::Vendored(_) => None,
-            },
-            (Self::SitePackages(root), FilePathRef::System(absolute_path)) => {
-                absolute_path.strip_prefix(root).ok().and_then(|path| {
-                    path.extension()
-                        .map_or(true, |ext| matches!(ext, "pyi" | "py"))
-                        .then_some(Self::SitePackages(path))
-                })
-            }
-            (Self::Extra(_), FilePathRef::Vendored(_)) => None,
-            (Self::FirstParty(_), FilePathRef::Vendored(_)) => None,
-            (Self::StandardLibrary(root), FilePathRef::Vendored(absolute_path)) => match root {
-                FilePathRef::System(_) => None,
-                FilePathRef::Vendored(root) => {
-                    absolute_path.strip_prefix(root).ok().and_then(|path| {
-                        path.extension()
-                            .map_or(true, |ext| ext == "pyi")
-                            .then_some(Self::StandardLibrary(FilePathRef::Vendored(path)))
-                    })
+            Self::StandardLibrary(path) => {
+                let parent = match path {
+                    StandardLibraryPath::System(system) => {
+                        StandardLibraryPath::System(system.parent()?)
+                    }
+                    StandardLibraryPath::Vendored(vendored) => {
+                        StandardLibraryPath::Vendored(vendored.parent()?)
+                    }
+                };
+                let parent_components = parent.components().map(|component| component.as_str());
+                let skip_final_part = match path {
+                    StandardLibraryPath::System(path) => path.ends_with("__init__.pyi"),
+                    StandardLibraryPath::Vendored(path) => path.ends_with("__init__.pyi"),
+                };
+                if skip_final_part {
+                    ModuleName::from_components(parent_components)
+                } else {
+                    ModuleName::from_components(parent_components.chain(path.file_stem()))
                 }
-            },
-            (Self::SitePackages(_), FilePathRef::Vendored(_)) => None,
+            }
         }
     }
 }
@@ -429,33 +480,6 @@
 pub(crate) struct ModuleResolutionPathRef<'a>(ModuleResolutionPathRefInner<'a>);
 
 impl<'a> ModuleResolutionPathRef<'a> {
-    #[must_use]
-    pub(crate) fn is_directory(
-        &self,
-        search_path: impl Into<Self>,
-        resolver: &ResolverState,
-    ) -> bool {
-        self.0.is_directory(search_path.into().0, resolver)
-    }
-
-    #[must_use]
-    pub(crate) fn is_regular_package(
-        &self,
-        search_path: impl Into<Self>,
-        resolver: &ResolverState,
-    ) -> bool {
-        self.0.is_regular_package(search_path.into().0, resolver)
-    }
-
-    #[must_use]
-    pub(crate) fn to_file(
-        self,
-        search_path: impl Into<Self>,
-        resolver: &ResolverState,
-    ) -> Option<File> {
-        self.0.to_file(search_path.into().0, resolver)
-    }
-
     #[must_use]
     pub(crate) fn to_module_name(self) -> Option<ModuleName> {
         self.0.to_module_name()
@@ -470,11 +494,6 @@
     pub(crate) fn with_py_extension(self) -> Option<ModuleResolutionPathBuf> {
         self.0.with_py_extension().map(ModuleResolutionPathBuf)
     }
-
-    #[must_use]
-    pub(crate) fn relativize_path(&self, absolute_path: &FilePathRef<'a>) -> Option<Self> {
-        self.0.relativize_path(absolute_path).map(Self)
-    }
 }
 
 impl fmt::Debug for ModuleResolutionPathRef<'_> {
@@ -508,10 +527,10 @@
                 ModuleResolutionPathRefInner::FirstParty(path)
             }
             ModuleResolutionPathBufInner::StandardLibrary(FilePath::System(path)) => {
-                ModuleResolutionPathRefInner::StandardLibrary(FilePathRef::System(path))
+                ModuleResolutionPathRefInner::StandardLibrary(StandardLibraryPath::System(path))
             }
             ModuleResolutionPathBufInner::StandardLibrary(FilePath::Vendored(path)) => {
-                ModuleResolutionPathRefInner::StandardLibrary(FilePathRef::Vendored(path))
+                ModuleResolutionPathRefInner::StandardLibrary(StandardLibraryPath::Vendored(path))
             }
             ModuleResolutionPathBufInner::SitePackages(path) => {
                 ModuleResolutionPathRefInner::SitePackages(path)
@@ -527,10 +546,12 @@
             ModuleResolutionPathRefInner::Extra(path) => path == other,
             ModuleResolutionPathRefInner::FirstParty(path) => path == other,
             ModuleResolutionPathRefInner::SitePackages(path) => path == other,
-            ModuleResolutionPathRefInner::StandardLibrary(FilePathRef::System(path)) => {
+            ModuleResolutionPathRefInner::StandardLibrary(StandardLibraryPath::System(path)) => {
                 path == other
             }
-            ModuleResolutionPathRefInner::StandardLibrary(FilePathRef::Vendored(_)) => false,
+            ModuleResolutionPathRefInner::StandardLibrary(StandardLibraryPath::Vendored(_)) => {
+                false
+            }
         }
     }
 }
@@ -559,8 +580,8 @@
             ModuleResolutionPathRefInner::Extra(_) => false,
             ModuleResolutionPathRefInner::FirstParty(_) => false,
             ModuleResolutionPathRefInner::SitePackages(_) => false,
-            ModuleResolutionPathRefInner::StandardLibrary(FilePathRef::System(_)) => false,
-            ModuleResolutionPathRefInner::StandardLibrary(FilePathRef::Vendored(path)) => {
+            ModuleResolutionPathRefInner::StandardLibrary(StandardLibraryPath::System(_)) => false,
+            ModuleResolutionPathRefInner::StandardLibrary(StandardLibraryPath::Vendored(path)) => {
                 path == other
             }
         }
@@ -715,7 +736,7 @@
 
         assert_eq!(
             ModuleResolutionPathRef(ModuleResolutionPathRefInner::StandardLibrary(
-                FilePathRef::system("foo.pyi")
+                StandardLibraryPath::system("foo.pyi")
             ))
             .to_module_name(),
             ModuleName::new_static("foo")
@@ -734,7 +755,7 @@
     fn module_name_2_parts() {
         assert_eq!(
             ModuleResolutionPathRef(ModuleResolutionPathRefInner::StandardLibrary(
-                FilePathRef::system("foo/bar")
+                StandardLibraryPath::system("foo/bar")
             ))
             .to_module_name(),
             ModuleName::new_static("foo.bar")
@@ -842,13 +863,13 @@
             ModuleResolutionPathBuf::standard_library(FilePath::system("foo/stdlib")).unwrap();
 
         // Must have a `.pyi` extension or no extension:
-        let bad_absolute_path = FilePathRef::system("foo/stdlib/x.py");
+        let bad_absolute_path = FilePath::system("foo/stdlib/x.py");
         assert_eq!(root.relativize_path(&bad_absolute_path), None);
-        let second_bad_absolute_path = FilePathRef::system("foo/stdlib/x.rs");
+        let second_bad_absolute_path = FilePath::system("foo/stdlib/x.rs");
         assert_eq!(root.relativize_path(&second_bad_absolute_path), None);
 
         // Must be a path that is a child of `root`:
-        let third_bad_absolute_path = FilePathRef::system("bar/stdlib/x.pyi");
+        let third_bad_absolute_path = FilePath::system("bar/stdlib/x.pyi");
         assert_eq!(root.relativize_path(&third_bad_absolute_path), None);
     }
 
@@ -856,10 +877,10 @@
     fn relativize_non_stdlib_path_errors() {
         let root = ModuleResolutionPathBuf::extra("foo/stdlib").unwrap();
         // Must have a `.py` extension, a `.pyi` extension, or no extension:
-        let bad_absolute_path = FilePathRef::system("foo/stdlib/x.rs");
+        let bad_absolute_path = FilePath::system("foo/stdlib/x.rs");
         assert_eq!(root.relativize_path(&bad_absolute_path), None);
         // Must be a path that is a child of `root`:
-        let second_bad_absolute_path = FilePathRef::system("bar/stdlib/x.pyi");
+        let second_bad_absolute_path = FilePath::system("bar/stdlib/x.pyi");
         assert_eq!(root.relativize_path(&second_bad_absolute_path), None);
     }
 
@@ -868,10 +889,10 @@
         assert_eq!(
             ModuleResolutionPathBuf::standard_library(FilePath::system("foo/baz"))
                 .unwrap()
-                .relativize_path(&FilePathRef::system("foo/baz/eggs/__init__.pyi"))
+                .relativize_path(&FilePath::system("foo/baz/eggs/__init__.pyi"))
                 .unwrap(),
             ModuleResolutionPathRef(ModuleResolutionPathRefInner::StandardLibrary(
-                FilePathRef::system("eggs/__init__.pyi")
+                StandardLibraryPath::system("eggs/__init__.pyi")
             ))
         );
     }
Index: crates/ruff_db/src/files/path.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_db/src/files/path.rs b/crates/ruff_db/src/files/path.rs
--- a/crates/ruff_db/src/files/path.rs	(revision 129b3b9ac3148b2fe1a93a7d83ef775f875dc26d)
+++ b/crates/ruff_db/src/files/path.rs	(date 1720429770237)
@@ -182,64 +182,3 @@
         other == self
     }
 }
-
-#[derive(Debug, Clone, Copy, PartialEq, Eq, Hash)]
-pub enum FilePathRef<'a> {
-    System(&'a SystemPath),
-    Vendored(&'a VendoredPath),
-}
-
-impl<'a> FilePathRef<'a> {
-    pub fn to_path_buf(self) -> FilePath {
-        match self {
-            Self::System(path) => FilePath::System(path.to_path_buf()),
-            Self::Vendored(path) => FilePath::Vendored(path.to_path_buf()),
-        }
-    }
-
-    pub fn parent(&self) -> Option<Self> {
-        match self {
-            Self::System(path) => path.parent().map(Self::System),
-            Self::Vendored(path) => path.parent().map(Self::Vendored),
-        }
-    }
-
-    pub fn components(&self) -> camino::Utf8Components {
-        match self {
-            Self::System(path) => path.components(),
-            Self::Vendored(path) => path.components(),
-        }
-    }
-
-    pub fn file_stem(&self) -> Option<&str> {
-        match self {
-            Self::System(path) => path.file_stem(),
-            Self::Vendored(path) => path.file_stem(),
-        }
-    }
-
-    #[inline]
-    pub fn to_file(self, db: &dyn Db) -> Option<File> {
-        match self {
-            Self::System(path) => system_path_to_file(db, path),
-            Self::Vendored(path) => vendored_path_to_file(db, path),
-        }
-    }
-
-    pub fn system(path: &'a (impl AsRef<SystemPath> + ?Sized)) -> Self {
-        Self::System(path.as_ref())
-    }
-
-    pub fn vendored(path: &'a (impl AsRef<VendoredPath> + ?Sized)) -> Self {
-        Self::Vendored(path.as_ref())
-    }
-}
-
-impl<'a> From<&'a FilePath> for FilePathRef<'a> {
-    fn from(value: &'a FilePath) -> Self {
-        match value {
-            FilePath::System(path) => FilePathRef::System(path),
-            FilePath::Vendored(path) => FilePathRef::Vendored(path),
-        }
-    }
-}
Index: crates/red_knot_module_resolver/src/resolver.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/red_knot_module_resolver/src/resolver.rs b/crates/red_knot_module_resolver/src/resolver.rs
--- a/crates/red_knot_module_resolver/src/resolver.rs	(revision 129b3b9ac3148b2fe1a93a7d83ef775f875dc26d)
+++ b/crates/red_knot_module_resolver/src/resolver.rs	(date 1720429722761)
@@ -1,7 +1,7 @@
 use std::ops::Deref;
 use std::sync::Arc;
 
-use ruff_db::files::{File, FilePath, FilePathRef};
+use ruff_db::files::{File, FilePath};
 use ruff_db::system::SystemPathBuf;
 
 use crate::db::Db;
@@ -78,14 +78,14 @@
 pub(crate) fn file_to_module(db: &dyn Db, file: File) -> Option<Module> {
     let _span = tracing::trace_span!("file_to_module", ?file).entered();
 
-    let path = FilePathRef::from(file.path(db.upcast()));
+    let path = file.path(db.upcast());
 
     let resolver_settings = module_resolver_settings(db);
 
     let relative_path = resolver_settings
         .search_paths()
         .iter()
-        .find_map(|root| root.relativize_path(&path))?;
+        .find_map(|root| root.relativize_path(path))?;
 
     let module_name = relative_path.to_module_name()?;
 
Index: crates/red_knot_module_resolver/src/typeshed/versions.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/red_knot_module_resolver/src/typeshed/versions.rs b/crates/red_knot_module_resolver/src/typeshed/versions.rs
--- a/crates/red_knot_module_resolver/src/typeshed/versions.rs	(revision 129b3b9ac3148b2fe1a93a7d83ef775f875dc26d)
+++ b/crates/red_knot_module_resolver/src/typeshed/versions.rs	(date 1720428655392)
@@ -8,7 +8,7 @@
 use once_cell::sync::Lazy;
 use rustc_hash::FxHashMap;
 
-use ruff_db::files::{system_path_to_file, File, FilePathRef};
+use ruff_db::files::{system_path_to_file, File, FilePath};
 use ruff_db::source::source_text;
 
 use crate::db::Db;
@@ -41,13 +41,13 @@
         &self,
         module: &ModuleName,
         db: &'db dyn Db,
-        stdlib_root: &FilePathRef,
+        stdlib_root: &FilePath,
         target_version: TargetVersion,
     ) -> TypeshedVersionsQueryResult {
         let versions = self.0.get_or_init(|| {
             let versions_path = match stdlib_root {
-                FilePathRef::System(path) => path.join("VERSIONS"),
-                FilePathRef::Vendored(_) => return &VENDORED_VERSIONS,
+                FilePath::System(path) => path.join("VERSIONS"),
+                FilePath::Vendored(_) => return &VENDORED_VERSIONS,
             };
             let Some(versions_file) = system_path_to_file(db.upcast(), &versions_path) else {
                 todo!(
Index: crates/ruff_db/src/vendored/path.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_db/src/vendored/path.rs b/crates/ruff_db/src/vendored/path.rs
--- a/crates/ruff_db/src/vendored/path.rs	(revision 129b3b9ac3148b2fe1a93a7d83ef775f875dc26d)
+++ b/crates/ruff_db/src/vendored/path.rs	(date 1720429040819)
@@ -42,7 +42,7 @@
     }
 
     #[must_use]
-    pub fn join(&self, other: impl AsRef<Utf8Path>) -> VendoredPathBuf {
+    pub fn join(&self, other: impl AsRef<VendoredPath>) -> VendoredPathBuf {
         VendoredPathBuf(self.0.join(other.as_ref()))
     }
 
@@ -63,7 +63,7 @@
 
     pub fn strip_prefix(
         &self,
-        prefix: impl AsRef<Utf8Path>,
+        prefix: impl AsRef<VendoredPath>,
     ) -> Result<&Self, path::StripPrefixError> {
         self.0.strip_prefix(prefix.as_ref()).map(Self::new)
     }
Index: crates/ruff_db/src/files.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_db/src/files.rs b/crates/ruff_db/src/files.rs
--- a/crates/ruff_db/src/files.rs	(revision 129b3b9ac3148b2fe1a93a7d83ef775f875dc26d)
+++ b/crates/ruff_db/src/files.rs	(date 1720429773624)
@@ -3,7 +3,7 @@
 use countme::Count;
 use dashmap::mapref::entry::Entry;
 
-pub use path::{FilePath, FilePathRef};
+pub use path::FilePath;
 
 use crate::file_revision::FileRevision;
 use crate::files::private::FileStatus;
```
</details>

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:479 on 2024-07-08 09:16_

I wonder if we could remove a lot of code if the structure would instead be:

```
enum ModuleResolutionPathInner {
    System { path: SystemPath, kind: ResolutionPathKind },
    vendored { path: VendoredPath }
}
```

because most of these branches have exactly the same code. The only difference is the variant taht they construct. But we can defer this for now because I think Carl's recommendation would also address this.

---

_@MichaReiser approved on 2024-07-08 09:18_

Overall looks good.

I would prefer if we can avoid introducing a `FilePathRef`, see the inline comment why and a solution on how we can avoid it. 

Overall, I start to like Carl's recommendation more and more. I think the current design makes some transformation unnecessarily verbose. But I'm okay moving forward with this (but would prefer for Carl to have a look at the PR as well)

---

_@AlexWaygood reviewed on 2024-07-08 09:22_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/typeshed/versions.rs`:45 on 2024-07-08 09:22_

ah, sorry, you mentioned that in your review of my last PR but I must have missed this instance :(

---

_@AlexWaygood reviewed on 2024-07-08 11:15_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/db.rs`:193 on 2024-07-08 11:15_

Hmm, this is a bit of a rabbit hole 😁 there's other refactors we could also do to the test suite more broadly that would make this nicer, but it makes the diff messier and bigger than is necessary here IMO... I think I'd prefer to defer this to a followup?

---

_@MichaReiser reviewed on 2024-07-08 11:19_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/db.rs`:193 on 2024-07-08 11:19_

Sounds good to me

---

_@AlexWaygood reviewed on 2024-07-08 11:37_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/typeshed/versions.rs`:89 on 2024-07-08 11:37_

That's correct, yeah. I don't feel strongly, but going via the vendored file system felt like unnecessary overhead/complexity in this case. We know exactly where the file is located relative to this file, and we know it's tiny (5KB), so this seemed simpler. But I'm happy to go via the vendored file system if you prefer.

---

_@MichaReiser reviewed on 2024-07-08 12:40_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/typeshed/versions.rs`:89 on 2024-07-08 12:40_

I prefer going over the vendored file system. 5KB isn't a lot, but it isn't nothing (IMO, not tiny). I don't think the overhead over the vendored file system is going to be significant in the grand scheme. 

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/path.rs`:378 on 2024-07-08 18:31_

This is doing exactly the same as in the above clause, why should it be structured differently (which obscures the fact that it's actually the same)?

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/path.rs`:435 on 2024-07-08 18:36_

It looks like this takes `absolute_path` as a `StandardLibraryPathRef`, not because it really must be a standard library path at all (in many cases we will have to smuggle some other kind of path claiming that it is a `StandardLibraryPathRef`), but simply because that's the only way we can make it possible to ever give a `VendoredPath`?

IMO this makes it super clear that we really do need a refactor of how paths are represented here, this is just a lie in the type signature of the function, and very confusing.

---

_@carljm approved on 2024-07-08 18:39_

---

_@AlexWaygood reviewed on 2024-07-08 19:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:435 on 2024-07-08 19:18_

Ah, you're right, this is indeed very confusing naming from me. I'll fix this, thanks.

(This is because it was originally envisaged as a general-purpose `FilePathRef` enum in `ruff_db`, but Micha asked me to move it to this crate for now. I renamed it without thinking too much about it when I moved it into this crate, but `FilePathRef` probably is better after all.)

Separately, I do still also agree that your suggested refactoring from the other PR sounds good, and still have it on my to-do list to look into it.

---

_@AlexWaygood reviewed on 2024-07-08 19:38_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:378 on 2024-07-08 19:38_

If I make this change:

```diff
--- a/crates/red_knot_module_resolver/src/path.rs
+++ b/crates/red_knot_module_resolver/src/path.rs
@@ -376,8 +376,10 @@ impl<'a> ModuleResolutionPathRefInner<'a> {
                 }
             }
             Self::StandardLibrary(path) => {
-                let parent = path.parent()?;
-                let parent_components = parent.components().map(|component| component.as_str());
+                let parent_components = path
+                    .parent()?
+                    .components()
+                    .map(|component| component.as_str());
```

The borrow checker complains at me:

```
error[E0716]: temporary value dropped while borrowed
   --> crates/red_knot_module_resolver/src/path.rs:379:41
    |
379 |                   let parent_components = path
    |  _________________________________________^
380 | |                     .parent()?
    | |______________________________^ creates a temporary value which is freed while still in use
381 |                       .components()
382 |                       .map(|component| component.as_str());
    |                                                           - temporary value is freed at the end of this statement
...
388 |                       ModuleName::from_components(parent_components)
    |                                                   ----------------- borrow later used here
    |
    = note: consider using a `let` binding to create a longer lived value
```

I can't really understand why it's okay with the first branch but not with the second. I guess I'll make the first branch more like the second.

---

_Merged by @AlexWaygood on 2024-07-09 09:21_

---

_Closed by @AlexWaygood on 2024-07-09 09:21_

---

_Branch deleted on 2024-07-09 09:21_

---
