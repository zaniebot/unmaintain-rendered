```yaml
number: 11982
title: "[red-knot] Use POSIX representations of paths when creating the typeshed zip file"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: normalize-zip-paths
created_at: 2024-06-22T13:35:17Z
updated_at: 2024-06-22T16:54:19Z
url: https://github.com/astral-sh/ruff/pull/11982
synced_at: 2026-01-12T15:55:40Z
```

# [red-knot] Use POSIX representations of paths when creating the typeshed zip file

---

_@AlexWaygood_

## Summary

This PR fixes the bug that caused the tests added in https://github.com/astral-sh/ruff/pull/11970 to fail. In the higher-level `ruff_db::vendored::VendoredFileSystem` abstraction we use for querying whether files exist in the zip file, all paths are normalized to use POSIX path separators before they're looked up in the underlying zip archive. But when we're creating the zip archive in `build.rs`, we're currently just using the operating system's default path separator (`\\` on Windows!). If we normalize the path separator in the `ruff_db` abstractions, we need to also do so in `build.rs`.

## Test Plan

`cargo test -p red_knot_module_resolver` still passes for me on macOS; hopefully it will also pass on Windows in CI. I'm pretty confident this is the cause of the test failures we saw on https://github.com/astral-sh/ruff/pull/11970: in a PR to my fork here, I added some debug prints so we could figure out what was going on with Windows, and you can see that the zip file on Windows has paths such as `stdlib\\abc.pyi` instead of `stdlib/abc.pyi`: https://github.com/AlexWaygood/ruff/pull/10.

---

_Label `red-knot` added by @AlexWaygood on 2024-06-22 13:35_

---

_Review requested from @carljm by @AlexWaygood on 2024-06-22 13:35_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-22 13:35_

---

_@MichaReiser reviewed on 2024-06-22 13:53_

Hmm I don't think that's necessary. At least according to the ZIP file standard:

>    4.4.17.1 The name of the file, with optional relative path.
       The path stored MUST NOT contain a drive or
       device letter, or a leading slash.  All slashes
       MUST be forward slashes '/' as opposed to
       backwards slashes '\' for compatibility with Amiga
       and UNIX file systems etc.  If input came from standard
       input, there is no file name field.
https://pkware.cachefly.net/webdocs/casestudies/APPNOTE.TXT

I wonder if all that is needed is to fix the test?

---

_Comment by @github-actions[bot] on 2024-06-22 13:54_

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

_Comment by @AlexWaygood on 2024-06-22 13:55_

Not sure I understand. I think that's all that I'm doing. All that this PR does is use `path_slash` to change `\`s to `/`s before writing the files to the zip archive at build time. No other normalization is being done here. What do you think I'm doing here that's unnecessary?

---

_@MichaReiser reviewed on 2024-06-22 14:00_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/build.rs`:43 on 2024-06-22 14:00_

There's a [`start_file_from_path`](https://docs.rs/zip/1.1.4/zip/write/struct.ZipWriter.html#method.start_file_from_path) function that takes care of the normalization

---

_@MichaReiser reviewed on 2024-06-22 14:02_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/typeshed.rs`:21 on 2024-06-22 14:02_

Rather than adding the dependency, I recommend to construct the path as a normal string: `"stdlib/functools.pyi"`, seems simpler and removes the need for a dependency

---

_@AlexWaygood reviewed on 2024-06-22 14:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/typeshed.rs`:21 on 2024-06-22 14:03_

Good point, this isn't needed here

---

_@AlexWaygood reviewed on 2024-06-22 14:05_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/build.rs`:43 on 2024-06-22 14:05_

On v0.6.6, the version we have pinned in `Cargo.toml`, there is a deprecation warning attached to this method in the docs: https://docs.rs/zip/0.6.6/zip/write/struct.ZipWriter.html#method.start_file_from_path.

The deprecation warning isn't there on the latest version, so I suppose they must have fixed the issues and undeprecated the method. But we're keeping `zip` pinned to the old version due to https://github.com/astral-sh/uv/issues/3642

---

_@AlexWaygood reviewed on 2024-06-22 14:12_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/build.rs`:43 on 2024-06-22 14:12_

This is what the change looks like (relative to this PR branch) if we want to keep the `zip` crate pinned to v0.6.6 and have Clippy pass:

```diff
diff --git a/crates/red_knot_module_resolver/build.rs b/crates/red_knot_module_resolver/build.rs
index 15f67f3bb..c138d3bc9 100644
--- a/crates/red_knot_module_resolver/build.rs
+++ b/crates/red_knot_module_resolver/build.rs
@@ -8,7 +8,6 @@
 use std::fs::File;
 use std::path::Path;
 
-use path_slash::PathExt;
 use zip::result::ZipResult;
 use zip::write::{FileOptions, ZipWriter};
 use zip::CompressionMethod;
@@ -30,24 +29,24 @@ fn zip_dir(directory_path: &str, writer: File) -> ZipResult<File> {
     for entry in walkdir::WalkDir::new(directory_path) {
         let dir_entry = entry.unwrap();
         let absolute_path = dir_entry.path();
-        let normalized_relative_path = absolute_path
+        let relative_path = absolute_path
             .strip_prefix(Path::new(directory_path))
-            .unwrap()
-            .to_slash()
-            .expect("Unexpected non-utf8 typeshed path!");
+            .unwrap();
 
         // Write file or directory explicitly
         // Some unzip tools unzip files with directory paths correctly, some do not!
         if absolute_path.is_file() {
-            println!("adding file {absolute_path:?} as {normalized_relative_path:?} ...");
-            zip.start_file(normalized_relative_path, options)?;
+            println!("adding file {absolute_path:?} as {relative_path:?} ...");
+            #[allow(deprecated)]
+            zip.start_file_from_path(relative_path, options)?;
             let mut f = File::open(absolute_path)?;
             std::io::copy(&mut f, &mut zip).unwrap();
-        } else if !normalized_relative_path.is_empty() {
+        } else if relative_path == Path::new("") {
             // Only if not root! Avoids path spec / warning
             // and mapname conversion failed error on unzip
-            println!("adding dir {absolute_path:?} as {normalized_relative_path:?} ...");
-            zip.add_directory(normalized_relative_path, options)?;
+            println!("adding dir {absolute_path:?} as {relative_path:?} ...");
+            #[allow(deprecated)]
+            zip.add_directory_from_path(relative_path, options)?;
         }
     }
     zip.finish()
```

---

_@MichaReiser approved on 2024-06-22 16:47_

---

_Merged by @AlexWaygood on 2024-06-22 16:54_

---

_Closed by @AlexWaygood on 2024-06-22 16:54_

---

_Branch deleted on 2024-06-22 16:54_

---
