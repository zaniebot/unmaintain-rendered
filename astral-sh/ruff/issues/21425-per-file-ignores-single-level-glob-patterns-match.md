```yaml
number: 21425
title: "`per-file-ignores` single-level glob patterns match files in subdirectories"
type: issue
state: closed
author: ponko2
labels: []
assignees: []
created_at: 2025-11-13T12:16:13Z
updated_at: 2025-11-13T23:23:43Z
url: https://github.com/astral-sh/ruff/issues/21425
synced_at: 2026-01-12T15:54:57Z
```

# `per-file-ignores` single-level glob patterns match files in subdirectories

---

_@ponko2_

### Summary

It seems that the `per-file-ignores` glob pattern matching does not fully respect directory boundaries when using single-level glob patterns such as `src/*.py`.  
A pattern like `src/*.py` is expected to match only files directly under the `src/` directory, but it currently also appears to match files in subdirectories like `src/subdir/`.

### Expected behavior

Only files directly under `src/` should be ignored.  
Files in `src/subdir/` should still be linted.

### Actual behavior

Both `src/ignored.py` and `src/subdir/selected.py` are ignored.  
This might not be the intended behavior.

### Minimal Reproducible Example

#### ruff.toml

```toml
[lint.per-file-ignores]
"src/*.py" = ["UP"]
```

#### File structure

```
selected.py
src/ignored.py
src/subdir/selected.py
```

#### Command

```bash
ruff check --config=ruff.toml --select=UP
```

#### Expected output

```
UP009 [*] UTF-8 encoding declaration is unnecessary
 --> selected.py:1:1
  |
1 | # -*- coding: utf-8 -*-
  | ^^^^^^^^^^^^^^^^^^^^^^^
  |
help: Remove unnecessary coding comment

UP009 [*] UTF-8 encoding declaration is unnecessary
 --> src/subdir/selected.py:1:1
  |
1 | # -*- coding: utf-8 -*-
  | ^^^^^^^^^^^^^^^^^^^^^^^
  |
help: Remove unnecessary coding comment

Found 2 errors.
```

#### Actual output

```
UP009 [*] UTF-8 encoding declaration is unnecessary
 --> selected.py:1:1
  |
1 | # -*- coding: utf-8 -*-
  | ^^^^^^^^^^^^^^^^^^^^^^^
  |
help: Remove unnecessary coding comment

Found 1 error.
```

### Possible fix

Using `GlobBuilder` with `literal_separator(true)` seems to correctly respect directory boundaries.

This approach is inspired by how `GlobBuilder` is used in Ruff's `ty_project` crate for exclude patterns:
https://github.com/astral-sh/ruff/blob/cd183c5e1f82e595ad717f844190948c7f109454/crates/ty_project/src/glob/exclude.rs?plain=1#L279-L283

```diff
diff --git a/crates/ruff_linter/src/settings/types.rs b/crates/ruff_linter/src/settings/types.rs
index 05cd13909..6722a60ec 100644
--- a/crates/ruff_linter/src/settings/types.rs
+++ b/crates/ruff_linter/src/settings/types.rs
@@ -6,7 +6,7 @@ use std::str::FromStr;
 use std::string::ToString;

 use anyhow::{Context, Result, bail};
-use globset::{Glob, GlobMatcher, GlobSet, GlobSetBuilder};
+use globset::{Glob, GlobBuilder, GlobMatcher, GlobSet, GlobSetBuilder};
 use log::debug;
 use pep440_rs::{VersionSpecifier, VersionSpecifiers};
 use ruff_db::diagnostic::DiagnosticFormat;
@@ -731,12 +731,21 @@ impl<T> CompiledPerFileList<T> {
             .into_iter()
             .map(|per_file_ignore| {
                 // Construct absolute path matcher.
-                let absolute_matcher = Glob::new(&per_file_ignore.absolute.to_string_lossy())
-                    .with_context(|| format!("invalid glob {:?}", per_file_ignore.absolute))?
-                    .compile_matcher();
+                let absolute_matcher =
+                    GlobBuilder::new(&per_file_ignore.absolute.to_string_lossy())
+                        .literal_separator(true)
+                        // No need to support Windows-style paths, so the backslash can be used an escape.
+                        .backslash_escape(true)
+                        .build()
+                        .with_context(|| format!("invalid glob {:?}", per_file_ignore.absolute))?
+                        .compile_matcher();

                 // Construct basename matcher.
-                let basename_matcher = Glob::new(&per_file_ignore.basename)
+                let basename_matcher = GlobBuilder::new(&per_file_ignore.basename)
+                    .literal_separator(true)
+                    // No need to support Windows-style paths, so the backslash can be used an escape.
+                    .backslash_escape(true)
+                    .build()
                     .with_context(|| format!("invalid glob {:?}", per_file_ignore.basename))?
                     .compile_matcher();
```

### Version

ruff 0.14.4

---

_Comment by @ntBre on 2025-11-13 23:23_

Thanks for the nice write up! I think this is very closely related to #6262 because we use most of the same globbing code for the `exclude` option too.

I think I'll close this one as a duplicate since there's already discussion there about revisiting our globbing behavior more generally.

---

_Closed by @ntBre on 2025-11-13 23:23_

---
