```yaml
number: 10484
title: "Fixes bug in `uv remove` when only comments exist"
type: pull_request
state: merged
author: bnorick
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-remove-comment-indent-bug
created_at: 2025-01-10T23:21:44Z
updated_at: 2025-01-11T01:07:03Z
url: https://github.com/astral-sh/uv/pull/10484
synced_at: 2026-01-10T11:44:52Z
```

# Fixes bug in `uv remove` when only comments exist

---

_Pull request opened by @bnorick on 2025-01-10 23:21_

## Summary

Fixes a bug when there are only comments in the dependencies section.

Basically, after one removes all dependencies, if there are remaining comments then the value unwrapped here https://github.com/astral-sh/uv/blob/c198e2233efaa037a9154fd7fe2625e4c78d976b/crates/uv-workspace/src/pyproject_mut.rs#L1309 is never properly initialized.
It's initialized to `None`, here https://github.com/astral-sh/uv/blob/c198e2233efaa037a9154fd7fe2625e4c78d976b/crates/uv-workspace/src/pyproject_mut.rs#L1256, but doesn't get set to `Some(...)` until the first dependency here https://github.com/astral-sh/uv/blob/c198e2233efaa037a9154fd7fe2625e4c78d976b/crates/uv-workspace/src/pyproject_mut.rs#L1276 and since we remove them all... there are none.

## Test Plan
Manually induced bug with
```
[project]
name = "t1"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "duct>=0.6.4",
    "minilog>=2.3.1",
    # comment
]
```

Then running
```
$ RUST_LOG=trace RUST_BACKTRACE=full uv remove duct minilog
DEBUG uv 0.5.8
DEBUG Found project root: `/home/bnorick/dev/workspace/t1`
DEBUG No workspace root found, using project root
thread 'main' panicked at crates/uv-workspace/src/pyproject_mut.rs:1294:73:
called `Option::unwrap()` on a `None` value
stack backtrace:
   0:     0x5638d7bed6ba - <unknown>
   1:     0x5638d783760b - <unknown>
   2:     0x5638d7bae232 - <unknown>
   3:     0x5638d7bf0f07 - <unknown>
   4:     0x5638d7bf215c - <unknown>
   5:     0x5638d7bf1972 - <unknown>
   6:     0x5638d7bf1909 - <unknown>
   7:     0x5638d7bf18f4 - <unknown>
   8:     0x5638d75087d2 - <unknown>
   9:     0x5638d750896b - <unknown>
  10:     0x5638d7508d68 - <unknown>
  11:     0x5638d8dcf1bb - <unknown>
  12:     0x5638d76be271 - <unknown>
  13:     0x5638d75ef1f9 - <unknown>
  14:     0x5638d75fc3cd - <unknown>
  15:     0x5638d772d9de - <unknown>
  16:     0x5638d8476812 - <unknown>
  17:     0x5638d83e1894 - <unknown>
  18:     0x5638d84722d3 - <unknown>
  19:     0x5638d83e1372 - <unknown>
  20:     0x7f851cfc7d90 - <unknown>
  21:     0x7f851cfc7e40 - __libc_start_main
  22:     0x5638d758e992 - <unknown>
  23:                0x0 - <unknown>
```

---

_Label `bug` added by @zanieb on 2025-01-10 23:32_

---

_Comment by @zanieb on 2025-01-10 23:42_

Thanks for the fix!

What do you think of the following change to avoid future regressions here?

```diff
diff --git a/crates/uv-workspace/src/pyproject_mut.rs b/crates/uv-workspace/src/pyproject_mut.rs
index b5c11c620..de45f70c6 100644
--- a/crates/uv-workspace/src/pyproject_mut.rs
+++ b/crates/uv-workspace/src/pyproject_mut.rs
@@ -1272,15 +1272,11 @@ fn reformat_array_multiline(deps: &mut Array) {
                 .map(|(s, _)| s)
                 .unwrap_or(decor_prefix);
 
-            // If there is no indentation, use four-space.
-            indentation_prefix = Some(if decor_prefix.is_empty() {
-                "    ".to_string()
-            } else {
-                decor_prefix.to_string()
-            });
+            indentation_prefix = (!decor_prefix.is_empty()).then_some(decor_prefix.to_string());
         }
 
-        let indentation_prefix_str = format!("\n{}", indentation_prefix.as_ref().unwrap());
+        let indentation_prefix_str =
+            format!("\n{}", indentation_prefix.as_deref().unwrap_or("    "));
 
         for comment in find_comments(decor.prefix()).chain(find_comments(decor.suffix())) {
             match comment.comment_type {
@@ -1307,7 +1303,7 @@ fn reformat_array_multiline(deps: &mut Array) {
                 match comment.comment_type {
                     CommentType::OwnLine => {
                         let indentation_prefix_str =
-                            format!("\n{}", indentation_prefix.as_ref().unwrap());
+                            format!("\n{}", indentation_prefix.as_deref().unwrap_or("    "));
                         rv.push_str(&indentation_prefix_str);
                     }
                     CommentType::EndOfLine => {
```

Would you mind adding a test case in `crates/uv/tests/it/edit.rs` too?

---

_Comment by @bnorick on 2025-01-11 00:17_

Done.

---

_@charliermarsh approved on 2025-01-11 00:36_

---

_Merged by @charliermarsh on 2025-01-11 01:07_

---

_Closed by @charliermarsh on 2025-01-11 01:07_

---
