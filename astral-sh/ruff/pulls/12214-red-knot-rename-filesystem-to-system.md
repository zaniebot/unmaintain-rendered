```yaml
number: 12214
title: "[red-knot] Rename `FileSystem` to `System`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: file-system-to-system
created_at: 2024-07-06T14:32:27Z
updated_at: 2024-07-09T07:31:48Z
url: https://github.com/astral-sh/ruff/pull/12214
synced_at: 2026-01-12T15:55:40Z
```

# [red-knot] Rename `FileSystem` to `System`

---

_@MichaReiser_

## Summary

This PR is mainly a renaming PR to address the confusion around the existing `FileSystem` trait. 

Today, Red Knot uses a `FileSystem` trait to abstract the system-dependent file-level operations. They're system-dependent because reading a file differs between the CLI (always goes to disk), the LSP (may return unsaved content), and WASM (has no file system). So far, this makes sense. However, it all became very confusing when we introduced the `VendoredFileSystem`, which doesn't implement `FileSystem` because the logic isn't system-dependent. Still, its name suggests that it should implement the `FileSystem` trait (it is a filesystem-like interface, after all).

This PR renames the `FileSystem` trait to `System` (and `VfsPath::FileSystem` to `VfsPath::System`, `FileSystemPath` to `SystemPath`, and `FileSystemPathBuf` to `SystemPathBuf`) to make the distinction more clear. The trait isn't about abstracting a way a file-system like API; it's solely for abstracting system-dependent logic, which, for now, is mainly IO-related, but I can envision more:

* Reading a file as a notebook rather than a string 
* Getting information about the runtime environment: CLI/LSP (OS, architecture, etc), WASM: wasm engine in addition to the OS architecture, etc
* Setting up watch tasks (I'm not 100% sure about this one yet, mainly because it requires tight integration into the host's main loop and, therefore, might not be possible to abstract away)
* Traversing a directory
* Getting the current working directory (again, WASM doesn't know the concept of current working directory)
* Getting the configuration directory


The second rename that I've done as part of this PR is to rename `Vfs` to `Files`, `VfsFile` to `File`, and `VfsPath` to `FilePath`. While it is correct that `Files` emulates a virtual file system by supporting both vendors and system paths, it seemed to me that it over-emphasizes that fact. As far as all upstream code is concerned, these are just files that Red Knot can operate on. That's why I opted for a simpler name.

### Should `VendoredFileSystem` and `System` have a shared `ReadOnlyFileSystem` trait?

I mulled over this for a long time. It is very intriguing: Both implement a file system-like API. However, I’m concluding that they should not have a shared base trait because:

- We have no use case today for a generic operation over a file system (either using `impl` or `dyn`). Such an operation would have the form of `fs.read_to_string(path)` where `fs` can either be `vendored` or `system`. 
- Even if the two have a shared base trait, they couldn’t be used interchangeably because `VendoredFileSystem` uses `VendoredPath` and `System` uses `SystemPath`. We could remove the distinction between `VendoredPath` and `SystemPath,` but I think this distinction is more valuable than having a shared base trait.
A shared base trait would widen the API of `VendoredFileSystem` because it suddenly must support `FileType::Symlink` and permissions.

I’m open to reconsidering introducing a shared base trait when we have a use case for it.


### Exposing the `VendoredFileSystem` 

Today, `VendoredFileSystem` is a field on `Files` (`Vfs`). This PR introduces a new `vendored` method on the `Db`. A trait method is necessary because the `ruff_db` function `source_text` and the `Files` struct operate on vendored files. but the vendored typeshed stubs only ship with `red_knot_module_resolver`. 

For tests, I decided that each test stores its own `VendoredFileSystem` instance. This allows replacing the `VendoredFileSystem` with an ad-hoc built zip file and also guarantees full isolation between tests. 

### Should `Db` implement `System`?

An alternative to what's done in this PR is to remove the `system` method on `Db` and instead make `Db` extend `System`. The main advantage is that it removes one additional dynamic-dispatch (`system()` returns `dyn System`). 

I opted for keeping the function because:

* It's not entirely clear to me yet if we'll have code that needs to be generic over system that runs before constructing `Program`. 
* It's easier to share code between the different runtime environments. We can have a single `Program` that takes a `&dyn System` as argument instead of having a custom `Program` for each environment (although we may still want to do that, this is unclear to me)

Overall, I think this can easily be changed later if we decide that `Db` extending `System` is the right way.


### Should it be named `Host`? 
An alternative to `System` is `Host`. I prefer `System` but don't have a strong reasoning for it. I'm open to renaming it to `Host` (or any other name)` if someone feels strongly. I prefer not to rename it if we all think both are about as good, just to safe some time.

## Test Plan

`cargo test`


---

_Comment by @codspeed-hq[bot] on 2024-07-06 14:48_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/file-system-to-system)

### Merging #12214 will **improve performances by 4.09%**

<sub>Comparing <code>file-system-to-system</code> (2047b82) with <code>main</code> (16a63c8)</sub>



### Summary

`⚡ 1` improvements
`✅ 32` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `file-system-to-system` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-with-preview-rules[unicode/pypinyin.py]` | 2.5 ms | 2.4 ms | +4.09% |


---

_Comment by @MichaReiser on 2024-07-06 15:15_

Hmm interesting. It seems that just including vendored file system causes a 60% perf regression because dropping it (the Zip has a ton of metadata) is so expensive.

---

_Comment by @AlexWaygood on 2024-07-06 15:22_

> Hmm interesting. It seems that just including vendored file system causes a 60% perf regression because dropping it (the Zip has a ton of metadata) is so expensive.

I was wondering about using a `Lazy` static variable for it, since it's immutable read-only data:

```diff
diff --git a/Cargo.lock b/Cargo.lock
index 1da9a1e54..f426a6cd2 100644
--- a/Cargo.lock
+++ b/Cargo.lock
@@ -1894,6 +1894,7 @@ dependencies = [
  "camino",
  "compact_str",
  "insta",
+ "once_cell",
  "path-slash",
  "ruff_db",
  "ruff_python_stdlib",
diff --git a/crates/red_knot_module_resolver/Cargo.toml b/crates/red_knot_module_resolver/Cargo.toml
index 99e69f35c..a6761665d 100644
--- a/crates/red_knot_module_resolver/Cargo.toml
+++ b/crates/red_knot_module_resolver/Cargo.toml
@@ -16,6 +16,7 @@ ruff_python_stdlib = { workspace = true }
 
 compact_str = { workspace = true }
 camino = { workspace = true }
+once_cell = { workspace = true }
 rustc-hash = { workspace = true }
 salsa = { workspace = true }
 tracing = { workspace = true }
diff --git a/crates/red_knot_module_resolver/src/lib.rs b/crates/red_knot_module_resolver/src/lib.rs
index d6ec501cc..0a1a77577 100644
--- a/crates/red_knot_module_resolver/src/lib.rs
+++ b/crates/red_knot_module_resolver/src/lib.rs
@@ -12,4 +12,4 @@ pub use module::{Module, ModuleKind};
 pub use module_name::ModuleName;
 pub use resolver::{resolve_module, set_module_resolution_settings, RawModuleResolutionSettings};
 pub use supported_py_version::TargetVersion;
-pub use typeshed::{TypeshedVersionsParseError, TypeshedVersionsParseErrorKind};
+pub use typeshed::{TypeshedVersionsParseError, TypeshedVersionsParseErrorKind, TYPESHED};
diff --git a/crates/red_knot_module_resolver/src/typeshed.rs b/crates/red_knot_module_resolver/src/typeshed.rs
index c8a36b462..718192d0d 100644
--- a/crates/red_knot_module_resolver/src/typeshed.rs
+++ b/crates/red_knot_module_resolver/src/typeshed.rs
@@ -1,22 +1,29 @@
 mod versions;
 
+use once_cell::sync::Lazy;
+
 pub(crate) use versions::{
     parse_typeshed_versions, LazyTypeshedVersions, TypeshedVersionsQueryResult,
 };
 pub use versions::{TypeshedVersionsParseError, TypeshedVersionsParseErrorKind};
 
+use ruff_db::vendored::VendoredFileSystem;
+
+// The file path here is hardcoded in this crate's `build.rs` script.
+// Luckily this crate will fail to build if this file isn't available at build time.
+const TYPESHED_ZIP_BYTES: &[u8] = include_bytes!(concat!(env!("OUT_DIR"), "/zipped_typeshed.zip"));
+
+pub static TYPESHED: Lazy<VendoredFileSystem> =
+    Lazy::new(|| VendoredFileSystem::new(TYPESHED_ZIP_BYTES).unwrap());
+
 #[cfg(test)]
 mod tests {
     use std::io::{self, Read};
     use std::path::Path;
 
-    use ruff_db::vendored::VendoredFileSystem;
-    use ruff_db::vfs::VendoredPath;
+    use ruff_db::vendored::path::VendoredPath;
 
-    // The file path here is hardcoded in this crate's `build.rs` script.
-    // Luckily this crate will fail to build if this file isn't available at build time.
-    const TYPESHED_ZIP_BYTES: &[u8] =
-        include_bytes!(concat!(env!("OUT_DIR"), "/zipped_typeshed.zip"));
+    use super::*;
 
     #[test]
     fn typeshed_zip_created_at_build_time() {
@@ -39,7 +46,6 @@ mod tests {
     #[test]
     fn typeshed_vfs_consistent_with_vendored_stubs() {
         let vendored_typeshed_dir = Path::new("vendor/typeshed").canonicalize().unwrap();
-        let vendored_typeshed_stubs = VendoredFileSystem::new(TYPESHED_ZIP_BYTES).unwrap();
 
         let mut empty_iterator = true;
         for entry in walkdir::WalkDir::new(&vendored_typeshed_dir).min_depth(1) {
@@ -58,16 +64,16 @@ mod tests {
                 .unwrap_or_else(|_| panic!("Expected {relative_path:?} to be valid UTF-8"));
 
             assert!(
-                vendored_typeshed_stubs.exists(vendored_path),
+                TYPESHED.exists(vendored_path),
                 "Expected {vendored_path:?} to exist in the `VendoredFileSystem`!
 
                 Vendored file system:
 
-                {vendored_typeshed_stubs:#?}
+                {TYPESHED:#?}
                 "
             );
 
-            let vendored_path_kind = vendored_typeshed_stubs
+            let vendored_path_kind = TYPESHED
                 .metadata(vendored_path)
                 .unwrap_or_else(|| {
                     panic!(
@@ -75,7 +81,7 @@ mod tests {
 
                         Vendored file system:
 
-                        {vendored_typeshed_stubs:#?}
+                        {TYPESHED:#?}
                         "
                     )
                 })
```

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/files/path.rs`:13 on 2024-07-06 15:23_

Maybe `FilePathBuf`, to emphasize the fact that the variants both contain owned data?

```suggestion
pub enum FilePathBuf {
```

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:161 on 2024-07-06 15:26_

In https://github.com/astral-sh/ruff/pull/11863#discussion_r1642716996, you said you "strongly" preferred not to implement `Default` for this struct ;) What made you change your mind on this? :)

---

_@AlexWaygood reviewed on 2024-07-06 15:26_

---

_@MichaReiser reviewed on 2024-07-06 15:37_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:161 on 2024-07-06 15:37_

Haha. I guess the only reason is that I needed an empty `VendoredFileSystem` to construct `TestDb`s

---

_@MichaReiser reviewed on 2024-07-06 15:38_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/files/path.rs`:13 on 2024-07-06 15:38_

I prefer to only use `Owned` `Ref`, and `Buf` for types that both have an owned and unowned representation because it otherwise implies that there are two types (a ref and owned). Here, we only have an owned type. 

---

_Comment by @MichaReiser on 2024-07-06 15:39_

> I was wondering about using a Lazy static variable for it, since it's immutable read-only data:

Hmm yeah, but we would also need to make it lazy in `Program` but it might be worth it. Anyway, I fixed the benchmarks by excluding the drop time :laughing: 

---

_Comment by @AlexWaygood on 2024-07-06 15:42_

> Hmm yeah, but we would also need to make it lazy in `Program`

I don't think `Program` would need to own a reference to it if it was just a module-level `static` variable. Code that needs to lookup a file in the vendored typeshed stubs could just reference the existing static variable from the module resolver crate.

---

_@AlexWaygood reviewed on 2024-07-06 15:46_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/files/path.rs`:13 on 2024-07-06 15:46_

I see that reasoning, but here I think it incorrectly implies that this is an unsized type, since most other `*Path` types (including all others in `ruff_db`) are unsized: `std::path::Path`, `camino::Utf8Path`, `SystemPath`, `VendoredPath`, etc.

I was also planning on using this enum in `red_knot_module_resolver::path.rs` for standard-library paths. I think there's a strong possibility that doing so might mean we'd need to add a `SystemPathRef` version of this enum. (But I suppose I could do the renaming then, as part of that PR, if I do go down that road.)

---

_@AlexWaygood reviewed on 2024-07-06 15:52_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:161 on 2024-07-06 15:52_

I guess I'm just not _necessarily_ sure it needs to be a field on `(Test)Db` at all

---

_@AlexWaygood reviewed on 2024-07-06 15:55_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:246 on 2024-07-06 15:55_

What's the motivation for changing the inner type from `&'static [u8]` to `Cow<&'static, [u8]>` here? Is this so that you can get rid of the `once_cell` dependency in the test code?

---

_@MichaReiser reviewed on 2024-07-06 15:59_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:246 on 2024-07-06 15:59_

No, it's so that tests can build an ad-hoc zip file (that doesn't have a `'static` lifetime) but that we can still avoid cloning the vendored data to the heap

---

_Comment by @MichaReiser on 2024-07-06 16:02_

> I don't think Program would need to own a reference to it if it was just a module-level static variable. Code that needs to lookup a file in the vendored typeshed stubs could just reference the existing static variable from the module resolver crate.

That's correct. That works for `Program` but not for the `TestDb` databases where each test should get their own instance. We could use `Cow` there as well, but I think the real solution is to just make `VendoredFileSystem` lazy by having an inner enum which is either `Raw(Cow<'static, [u8]>)` or `Zip(ZipArchive...)`. This way, the complexity is isolated into the `VendoredFileSystem` implementation without concerning the downstream code. I don't plan on changing this as part of this PR.

---

_Comment by @AlexWaygood on 2024-07-06 16:12_

Why does each test need to have its own `VendoredFileSystem` instance? I thought we agreed that using `--custom-typeshed-dir` and the `MemoryFileSystem` was sufficient for mocking out typeshed in tests. I expect only integration tests to need to look up files from a `VendoredFileSystem`, except for the small number of tests we already have in `ruff_db` that are focused unit tests for the specific functionality of the `VendoredFileSystem` itself

---

_Comment by @MichaReiser on 2024-07-06 17:04_

>  I thought we agreed that using --custom-typeshed-dir and the MemoryFileSystem was sufficient for mocking out typeshed in test

I understood that we agreed on that the ability to create ad-hoc zip files AND custom typeshed dir is sufficient. The former requires that tests have their own `VendoredFileSystem`. 

There are also a few tests in `ruff_db` where we don't have the stubbed vendored files.

I don't think that storing a `&'static VendoredFileSystem` on `Program` would solve the initialization cost. The zip file still gets loaded eagerly when creating the `Program`. The main advantage that I see is that dropping a `Program` doesn't require deallocating the `VendoredFileSystem`. But we shouldn't make too big a deal out of it. The Zip file doesn't store that much data, at least not compared to the amount of data that we'll store when loading builtins. 

The only other solution is to not store `vendored` on `Program` and instead make all code directly access a global static. I prefer not to have global variables because they make things harder to test. But more fundamentally, there's code in `ruff_db` that needs access to the vendored file system but the typeshed files are only available in `red_knot_module_resolver` and higher.

Either way. I prefer not to solve this as part of this PR.

---

_@MichaReiser reviewed on 2024-07-06 17:04_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:161 on 2024-07-06 17:04_

There are tests in `ruff_db` where it has to be a field. 

---

_@MichaReiser reviewed on 2024-07-06 17:08_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/files/path.rs`:13 on 2024-07-06 17:08_

Do mean a `FilePathRef`?.  

We can look into this when we need it. For now, we don't, and I think the refactor that @carljm proposed might also remove the need for it.

I also consider this out of the scope of this (draft) PR.

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/files/path.rs`:13 on 2024-07-06 17:10_

> Do mean a `FilePathRef`?

Yes! Sorry

> We can look into this when we need it

SGTM

---

_@AlexWaygood reviewed on 2024-07-06 17:10_

---

_@AlexWaygood reviewed on 2024-07-06 17:14_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/files/path.rs`:13 on 2024-07-06 17:14_

Though I still feel like calling it `FilePath` makes it inconsistent with `std::path::Path`, `camino::Utf8Path`, `SystemPath` and `VendoredPath`, which are all unsized types

---

_Comment by @AlexWaygood on 2024-07-06 18:08_

> Either way. I prefer not to solve this as part of this PR.

Sure — the reason for my questions was I was trying to understand the motivation for the changes you _were_ making as part of this PR, which might not necessarily be desirable if we went in a slightly different direction :-)

But I agree we don't need to come to a conclusive answer on these questions now!

---

_@MichaReiser reviewed on 2024-07-07 07:38_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/files/path.rs`:13 on 2024-07-07 07:38_

I agree that there's some inconsistency, but the opposite is also true: Naming it `PathBuf` implies the existence of `Path` and that would be inconsistent.

I'll resolve this. I only renamed the type from `VfsPath` to `FilePath` and I think we're over bikeshedding on the name here.

---

_Label `red-knot` added by @MichaReiser on 2024-07-07 08:05_

---

_@MichaReiser reviewed on 2024-07-07 08:51_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/memory_fs.rs`:1 on 2024-07-07 08:51_

This is now mainly a building block for systems that don't want to use a real fs (or can't because it doesn't exist). 

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:1 on 2024-07-07 08:53_

The main changes here are:

* The `FileSystem` is now wrapped in an `Arc` to implement `snapshot` (necessary for `ParallelDatabase`)
* I created some inline function to reduce monomorphization 
* Added a `VendoredFileSystemBuilder` that can be used in tests

---

_@MichaReiser reviewed on 2024-07-07 08:53_

---

_Marked ready for review by @MichaReiser on 2024-07-07 09:06_

---

_Review requested from @carljm by @MichaReiser on 2024-07-07 09:06_

---

_Comment by @github-actions[bot] on 2024-07-07 09:18_

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

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:9 on 2024-07-07 19:29_

Nit: I'd prefer these imports to be below the module docstring and below the standard-library imports

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/state.rs`:4 on 2024-07-07 19:33_

nit

```suggestion
use ruff_db::system::System;

use crate::db::Db;
use crate::supported_py_version::TargetVersion;
use crate::typeshed::LazyTypeshedVersions;
```

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/typeshed.rs`:3 on 2024-07-07 19:34_

```suggestion
use ruff_db::vendored::VendoredFileSystem;

```

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:4 on 2024-07-07 19:45_

nit: there's now some places in this file where we refer to `std::io::Cursor` as `io::Cursor`, and other places in this file where we just refer to it as `Cursor`. It would be nice to be consistent inside this module.

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:46 on 2024-07-07 19:47_

nit

```suggestion
            inner: Arc::clone(self.inner),
```

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:13 on 2024-07-07 19:53_

I think I'd prefer to do something like this:

```diff
diff --git a/crates/ruff_db/src/files.rs b/crates/ruff_db/src/files.rs
index 20d48a954..0510df95d 100644
--- a/crates/ruff_db/src/files.rs
+++ b/crates/ruff_db/src/files.rs
@@ -257,7 +257,7 @@ mod tests {
     use crate::files::{system_path_to_file, vendored_path_to_file};
     use crate::system::DbWithTestSystem;
     use crate::tests::TestDb;
-    use crate::vendored::VendoredFileSystemBuilder;
+    use crate::vendored::tests::VendoredFileSystemBuilder;
 
     #[test]
     fn file_system_existing_file() -> crate::system::Result<()> {
diff --git a/crates/ruff_db/src/parsed.rs b/crates/ruff_db/src/parsed.rs
index 3b5e1d575..fec144858 100644
--- a/crates/ruff_db/src/parsed.rs
+++ b/crates/ruff_db/src/parsed.rs
@@ -76,7 +76,8 @@ mod tests {
     use crate::parsed::parsed_module;
     use crate::system::{DbWithTestSystem, SystemPath};
     use crate::tests::TestDb;
-    use crate::vendored::{VendoredFileSystemBuilder, VendoredPath};
+    use crate::vendored::VendoredPath;
+    use crate::vendored::tests::VendoredFileSystemBuilder;
 
     #[test]
     fn python_file() -> crate::system::Result<()> {
diff --git a/crates/ruff_db/src/vendored.rs b/crates/ruff_db/src/vendored.rs
index 1bed7443d..2a2a47b01 100644
--- a/crates/ruff_db/src/vendored.rs
+++ b/crates/ruff_db/src/vendored.rs
@@ -8,9 +8,7 @@ use zip::{read::ZipFile, ZipArchive, ZipWriter};
 
 use crate::file_revision::FileRevision;
 
-pub use self::path::{VendoredPath, VendoredPathBuf};
-#[cfg(test)]
-pub use self::tests::VendoredFileSystemBuilder;
+pub use path::{VendoredPath, VendoredPathBuf};
 
 mod path;
 
@@ -340,7 +338,7 @@ impl<'a> From<&'a VendoredPath> for NormalizedVendoredPath<'a> {
 }
 
 #[cfg(test)]
-mod tests {
+pub(crate) mod tests {
     use std::io::Write;
 
     use insta::assert_snapshot;
```

---

_@AlexWaygood approved on 2024-07-07 19:55_

After playing around with this and thinking about it some more, this LGTM. Thanks for talking it through with me! Some minor nits below, but nothing blocking:

---

_@MichaReiser reviewed on 2024-07-08 10:07_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:9 on 2024-07-08 10:07_

Makes sense, but this is not a module level comment. I'll fix it.

---

_@MichaReiser reviewed on 2024-07-08 10:10_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/typeshed.rs`:3 on 2024-07-08 10:10_

RustRover disagrees with this. I need to use the `self::` prefix to get the desired sorting

---

_Comment by @MichaReiser on 2024-07-08 10:19_

I'll wait with merging to give @carljm a chance to look at it as well.

---

_Review comment by @carljm on `crates/ruff_db/src/system.rs`:38 on 2024-07-08 17:55_

Now that this is `System` and not `FileSystem`, I think `exists()` is too vague a method name:

```suggestion
    fn path_exists(&self, path: &SystemPath) -> bool {
```

---

_Review comment by @carljm on `crates/red_knot/src/main.rs`:38 on 2024-07-08 17:56_

rename this variable from `fs` to `sys` or `system`?

---

_Review comment by @carljm on `crates/ruff_db/src/system.rs`:32 on 2024-07-08 18:02_

Similar to below:
```suggestion
    fn path_metadata(&self, path: &SystemPath) -> Result<Metadata>;
```

---

_@carljm approved on 2024-07-08 18:02_

I definitely think this renaming makes things clearer!

---

_Merged by @MichaReiser on 2024-07-09 07:20_

---

_Closed by @MichaReiser on 2024-07-09 07:20_

---

_Branch deleted on 2024-07-09 07:20_

---
