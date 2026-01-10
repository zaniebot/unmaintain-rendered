```yaml
number: 20264
title: "[`flake8-async`] Implement `blocking-path-method` (`ASYNC240`)"
type: pull_request
state: merged
author: PieterCK
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-rule-async-240-blocking-path-usage
created_at: 2025-09-05T10:08:09Z
updated_at: 2025-09-23T14:42:03Z
url: https://github.com/astral-sh/ruff/pull/20264
synced_at: 2026-01-10T17:40:28Z
```

# [`flake8-async`] Implement `blocking-path-method` (`ASYNC240`)

---

_Pull request opened by @PieterCK on 2025-09-05 10:08_

## Summary
Adds a new rule to find and report use of `os.path` or `pathlib.Path` in
async functions.

Issue: #8451

## Test Plan

Using `cargo insta test`


---

_Marked ready for review by @PieterCK on 2025-09-05 10:24_

---

_Comment by @ntBre on 2025-09-05 12:52_

Thanks! We're pretty focused on the minor release this week, so I may not get a chance to review until next week. But this looks really good from a quick skim :)

---

_Label `rule` added by @ntBre on 2025-09-05 12:53_

---

_Label `preview` added by @ntBre on 2025-09-05 12:53_

---

_Review requested from @ntBre by @ntBre on 2025-09-05 12:53_

---

_Comment by @github-actions[bot] on 2025-09-05 13:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:132 on 2025-09-05 15:54_

I'm not sure what the upstream rule does, but IMO this should also have either an allow list or an exclusion list, because there are a number of methods on Path objects that aren't blocking — at the very least, we should allow use [PurePath methods](https://docs.python.org/3/library/pathlib.html#methods-and-properties) or functions like `os.path.join` that don't care about the actual filesystem.

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:188 on 2025-09-05 15:55_

IIRC, the creation of a Path object itself isn't blocking, so this entire function can probably just be `false`;

---

_@amyreese reviewed on 2025-09-05 15:59_

---

_@amyreese reviewed on 2025-09-05 16:01_

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:188 on 2025-09-05 16:01_

Although on second thought that might not catch calls like `os.path.exists(thing)`, so maybe instead make sure that eg, `pathlib.Path(...)` is allowed while `pathlib.Path(...).exists()` isn't.

---

_@ntBre reviewed on 2025-09-05 18:10_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:188 on 2025-09-05 18:10_

I think this is only used within the `check_type` infrastructure to check that the type of a variable (`p` in this case) is a `pathlib.Path` in a method call like:

```py
p = Path(".")
p.exists()
```

There's no diagnostic on line 49 of the test file, for example. So I think this is implemented correctly.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:188 on 2025-09-05 18:14_

Oh, I see what you mean. Yeah, I don't think we need to call `is_blocking_path_method` inside this method. We just need to check that it's a `Path`, which I think we might already have a `TypeChecker` for:

https://github.com/astral-sh/ruff/blob/16d674e63264acdf640349c81ec6263e813a3234/crates/ruff_python_semantic/src/analyze/typing.rs#L914-L917

This checks some sub/supeclasses too, we just need to double-check if we want that.

---

_@ntBre reviewed on 2025-09-05 18:14_

---

_Converted to draft by @PieterCK on 2025-09-08 15:46_

---

_Review comment by @PieterCK on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:132 on 2025-09-09 09:11_

The rule only mentions `os.path`, but yeah this makes sense. (I haven't looked into how they actually implemented it, though)

---

_@PieterCK reviewed on 2025-09-09 09:11_

---

_@PieterCK reviewed on 2025-09-09 09:15_

---

_Review comment by @PieterCK on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:132 on 2025-09-09 09:15_

I've added an allow list for this (see `maybe_calling_io_operation`). It's probably not complete, but safer than an incomplete exclusion list of I/O methods.

---

_@PieterCK reviewed on 2025-09-09 09:23_

---

_Review comment by @PieterCK on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:188 on 2025-09-09 09:23_

Ok! Switched to using `PathlibPathChecker`:
```diff
@@ -130,7 +168,14 @@ pub(crate) fn blocking_os_path(checker: &Checker, call: &ExprCall) {
         return;
     };
 
-    if check_type::<PathMethodInAsyncChecker>(binding, semantic) {
-        checker.report_diagnostic(BlockingPathMethodInAsyncFunction, call.func.range());
+    if check_type::<PathlibPathChecker>(binding, semantic) {
+        if maybe_calling_io_operation(attr.id.as_str()) {
+            checker.report_diagnostic(
+                BlockingPathMethodInAsyncFunction {
+                    path_library: "pathlib.Path".to_string(),
+                },
+                call.func.range(),
+            );
+        }
     }
```

Its `match_annotation` is missing a logic that checks if `Path` is in union and optional. Added that logic in this commit: **`Update PathlibPathChecker to check nested annotation.`**

---

_Review comment by @PieterCK on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:188 on 2025-09-09 09:42_

> This checks some sub/supeclasses too, we just need to double-check if we want that.

There are some classes in that check that aren't relevant for checking `ASYNC240`; `PurePath`, `PurePosixPath` and `PureWindowsPath`. They all don't have I/O related methods.

Not excluding them from that check shouldn't be a problem though, expressions calling those objects methods will still be filtered out by `maybe_calling_io_operation` since they have exactly the same method as `PurePath`.

---

_@PieterCK reviewed on 2025-09-09 09:42_

---

_@PieterCK reviewed on 2025-09-09 09:49_

---

_Review comment by @PieterCK on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:100 on 2025-09-09 09:49_

I've manually inspected these methods. They only do basic computation and don’t use any functions that `join` doesn’t use, so they’re probably safe.

---

_@PieterCK reviewed on 2025-09-09 09:54_

---

_Review comment by @PieterCK on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:90 on 2025-09-09 09:54_

This list is generated using a simple python (3.10.12) script:
```python
for attr in dir(pathlib.PurePath):
    if not attr.startswith("_"):
        print(f'"{attr}",')
```

---

_Renamed from "[flake8-async] Implement `blocking-path-method` (ASYNC240)." to "wip [flake8-async] Implement `blocking-path-method` (ASYNC240)." by @PieterCK on 2025-09-09 11:48_

---

_@PieterCK reviewed on 2025-09-09 12:15_

---

_Review comment by @PieterCK on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:188 on 2025-09-09 12:15_

> Although on second thought that might not catch calls like `os.path.exists(thing)`, so maybe instead make sure that eg, `pathlib.Path(...)` is allowed while `pathlib.Path(...).exists()` isn't.

The linter should be able to handle these cases now.




---

_Marked ready for review by @PieterCK on 2025-09-09 13:04_

---

_Comment by @PieterCK on 2025-09-09 13:04_

Thanks for reviewing this! Marking this as ready for review again. What's the best way to show new PR changes here? I've updated the existing commit this time.

The main changes are around how the linter determines whether an expression is an `os.path` or `pathlib.Path` method call that performs I/O. Previously, even constructing a `Path` object in async function triggers violation. Comment thread: https://github.com/astral-sh/ruff/pull/20264#discussion_r2325468204


<details>

<summary>

git diff of the `blocking_path_methods.rs` file:</summary>

```diff
git diff be73d44f8b10 c2125edbbb8d -- crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs
diff --git a/crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs b/crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs
index c67ce67a8..50e7059bc 100644
--- a/crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs
+++ b/crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs
@@ -1,8 +1,8 @@
 use crate::Violation;
 use crate::checkers::ast::Checker;
 use ruff_macros::{ViolationMetadata, derive_message_formats};
-use ruff_python_ast::{self as ast, Expr, ExprCall};
-use ruff_python_semantic::analyze::typing::{TypeChecker, check_type, traverse_union_and_optional};
+use ruff_python_ast::{self as ast, ExprCall};
+use ruff_python_semantic::analyze::typing::{PathlibPathChecker, check_type};
 use ruff_text_size::Ranged;
 
 /// ## What it does
@@ -35,68 +35,70 @@ use ruff_text_size::Ranged;
 ///     file_exists = await path.exists()
 /// ```
 #[derive(ViolationMetadata)]
-pub(crate) struct BlockingPathMethodInAsyncFunction;
+pub(crate) struct BlockingPathMethodInAsyncFunction {
+    path_library: String,
+}
 
 impl Violation for BlockingPathMethodInAsyncFunction {
     #[derive_message_formats]
     fn message(&self) -> String {
-        "Async functions should not use pathlib.Path or os.path methods, use trio.Path or anyio.path".to_string()
+        format!(
+            "Async functions should not use {path_library} methods, use trio.Path or anyio.path",
+            path_library = self.path_library
+        )
     }
 }
 
-const OS_PATH: [&str; 2] = ["os", "path"];
-const PATHLIB_PATH: [&str; 2] = ["pathlib", "Path"];
-
-fn is_blocking_path_method(segments: &[&str]) -> bool {
-    segments.starts_with(&OS_PATH) || segments.starts_with(&PATHLIB_PATH)
-}
-
-struct PathMethodInAsyncChecker;
-
-impl TypeChecker for PathMethodInAsyncChecker {
-    fn match_annotation(
-        annotation: &ruff_python_ast::Expr,
-        semantic: &ruff_python_semantic::SemanticModel,
-    ) -> bool {
-        // match base annotation directly
-        if semantic
-            .resolve_qualified_name(annotation)
-            .is_some_and(|qualified_name| is_blocking_path_method(qualified_name.segments()))
-        {
-            return true;
-        }
-
-        // otherwise traverse any union or optional annotation
-        let mut found = false;
-        traverse_union_and_optional(
-            &mut |inner_expr, _| {
-                if semantic
-                    .resolve_qualified_name(inner_expr)
-                    .is_some_and(|qualified_name| {
-                        is_blocking_path_method(qualified_name.segments())
-                    })
-                {
-                    found = true;
-                }
-            },
-            semantic,
-            annotation,
-        );
-        found
+fn is_calling_os_path_method(segments: &[&str]) -> bool {
+    if segments.len() != 3 {
+        return false;
     }
+    let Some(symbol_name) = segments.get(..2) else {
+        return false;
+    };
+    matches!(symbol_name, ["os", "path"])
+}
 
-    fn match_initializer(
-        initializer: &ruff_python_ast::Expr,
-        semantic: &ruff_python_semantic::SemanticModel,
-    ) -> bool {
-        let Expr::Call(ExprCall { func, .. }) = initializer else {
-            return false;
-        };
-
-        semantic
-            .resolve_qualified_name(func)
-            .is_some_and(|qualified_name| is_blocking_path_method(qualified_name.segments()))
-    }
+fn maybe_calling_io_operation(attr: &str) -> bool {
+    !matches!(
+        attr,
+        // Pure path objects provide path-handling operations which don’t actually
+        // access a filesystem.
+        // https://docs.python.org/3/library/pathlib.html#pure-paths
+        // pathlib.PurePath methods and properties:
+        "anchor"
+            | "as_posix"
+            | "as_uri"
+            | "drive"
+            | "is_absolute"
+            | "is_relative_to"
+            | "is_reserved"
+            | "joinpath"
+            | "match"
+            | "name"
+            | "parent"
+            | "parents"
+            | "parts"
+            | "relative_to"
+            | "root"
+            | "stem"
+            | "suffix"
+            | "suffixes"
+            | "with_name"
+            | "with_segments"
+            | "with_stem"
+            | "with_suffix"
+            // Non I/O pathlib.Path or os.path methods:
+            | "join"
+            | "dirname"
+            | "basename"
+            | "splitroot"
+            | "splitdrive"
+            | "splitext"
+            | "split"
+            | "isabs"
+            | "normcase"
+    )
 }
 
 /// ASYNC240
@@ -106,22 +108,53 @@ pub(crate) fn blocking_os_path(checker: &Checker, call: &ExprCall) {
         return;
     }
 
-    // Check if an expression is directly calling one of the blocking `path`
-    // objects.
+    // Check if an expression is calling I/O related os.path method.
+    // Just intializing pathlib.Path object is OK, we can return
+    // early in that scenario.
     if let Some(qualified_name) = semantic.resolve_qualified_name(call.func.as_ref()) {
         let segments = qualified_name.segments();
-        if is_blocking_path_method(segments) {
-            checker.report_diagnostic(BlockingPathMethodInAsyncFunction, call.func.range());
+        if !is_calling_os_path_method(segments) {
+            return;
+        }
+
+        let Some(os_path_method) = segments.last() else {
+            return;
+        };
+
+        if maybe_calling_io_operation(os_path_method) {
+            checker.report_diagnostic(
+                BlockingPathMethodInAsyncFunction {
+                    path_library: "os.path".to_string(),
+                },
+                call.func.range(),
+            );
         }
         return;
     }
 
-    // Past this, we're checking if a variable contains one of the blocking
-    // `path` objects.
-    let Some(ast::ExprAttribute { value, .. }) = call.func.as_attribute_expr() else {
+    // Check if an expression is a pathlib.Path constructor that directly
+    // calls an I/O method.
+    let Some(ast::ExprAttribute { value, attr, .. }) = call.func.as_attribute_expr() else {
         return;
     };
 
+    if let Some(ExprCall { func, .. }) = value.as_call_expr() {
+        if !PathlibPathChecker::is_pathlib_path_constructor(semantic, func) {
+            return;
+        }
+        if maybe_calling_io_operation(attr.id.as_str()) {
+            checker.report_diagnostic(
+                BlockingPathMethodInAsyncFunction {
+                    path_library: "pathlib.Path".to_string(),
+                },
+                call.func.range(),
+            );
+        }
+        return;
+    }
+
+    // Lastly, check if a variable is a pathlib.Path instance and it's
+    // calling an I/O method.
     let Some(name) = value.as_name_expr() else {
         return;
     };
@@ -130,7 +163,14 @@ pub(crate) fn blocking_os_path(checker: &Checker, call: &ExprCall) {
         return;
     };
 
-    if check_type::<PathMethodInAsyncChecker>(binding, semantic) {
-        checker.report_diagnostic(BlockingPathMethodInAsyncFunction, call.func.range());
+    if check_type::<PathlibPathChecker>(binding, semantic) {
+        if maybe_calling_io_operation(attr.id.as_str()) {
+            checker.report_diagnostic(
+                BlockingPathMethodInAsyncFunction {
+                    path_library: "pathlib.Path".to_string(),
+                },
+                call.func.range(),
+            );
+        }
     }
 }

```
</details>

---

_Renamed from "wip [flake8-async] Implement `blocking-path-method` (ASYNC240)." to "[flake8-async] Implement `blocking-path-method` (ASYNC240)." by @PieterCK on 2025-09-09 13:04_

---

_Review requested from @amyreese by @PieterCK on 2025-09-10 06:19_

---

_@jakkdl reviewed on 2025-09-10 10:48_

---

_Review comment by @jakkdl on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:132 on 2025-09-10 10:48_

The upstream implementation is not very fancy: https://github.com/python-trio/flake8-async/blob/d4c412ecc2ec2d4bdcd3908e0769adf60c1054ed/flake8_async/visitors/visitor2xx.py#L338

---

_Comment by @PieterCK on 2025-09-17 05:51_

Bumping this PR for review again.

---

_Comment by @ntBre on 2025-09-17 16:24_

> What's the best way to show new PR changes here?

I usually just push additional commits. That works better with GitHub's "Changes since your last review" button than comparing rebased commits. We always squash and merge PRs anyway. But it's not too big of a deal either way :) 

I'll give this another look now!

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:59 on 2025-09-17 16:31_

I think we can combine these three checks into one pattern match:


```suggestion
    matches!(segments, ["os", "path", _])
```

If we wanted to accept more than 3 components we could just use `..` instead of `_`. I don't think that helps here, just another neat trick.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:9 on 2025-09-17 16:33_

I think we should clarify here and in the `Violation::message` implementation that only blocking functions are disallowed now that the modules as a whole aren't flagged.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:65 on 2025-09-17 16:34_

nit: We usually put the main rule implementation function just below the `Violation` stuff and then helper functions after it.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:63 on 2025-09-17 16:39_

Are all of these shared between both `os.path` and `pathlib.Path`? I know sometimes those two modules have different names for similar functionality, but I didn't check all of these myself so it could be correct.

I'm also wondering if a deny list might be shorter than this allow list. If they're of comparable length, this is fine, just an idea.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:144 on 2025-09-17 16:51_

It looks like if you use `PathlibPathChecker::match_initializer` instead of `is_pathlib_path_constructor`, it will handle the `ExprCall` check for you too:

https://github.com/astral-sh/ruff/blob/cb3c3ba94d329083abf430072d740acf2aa476c7/crates/ruff_python_semantic/src/analyze/typing.rs#L919-L925

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:167 on 2025-09-17 16:52_

This is the same `attr` as above right? I think we could do this check once before both the constructor and `check_type` checks, returning early if it's not one of the known attributes.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC240.py`:78 on 2025-09-17 17:06_

I think we need to exclude `open` because it's covered by [blocking-open-call-in-async-function (ASYNC230)](https://docs.astral.sh/ruff/rules/blocking-open-call-in-async-function/#blocking-open-call-in-async-function-async230).

---

_Review comment by @ntBre on `crates/ruff_python_semantic/src/analyze/typing.rs`:929 on 2025-09-17 17:13_

I do like having this check like in #20091, but I'm a little bit hesitant to add it here since it will affect other rules too. I would probably either revert this and accept that we miss option/union annotations for now, until we possibly update the `TypeChecker` infrastructure more generally in the future, or move this check into the rule itself, as @amyreese did for `ASYNC212`.

---

_@ntBre reviewed on 2025-09-17 17:16_

Thank you! This looks great to me overall, I just had a few minor comments. I especially liked the tests. They were really easy to read and review 😄 

---

_@PieterCK reviewed on 2025-09-18 09:46_

---

_Review comment by @PieterCK on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:63 on 2025-09-18 09:46_

The deny list version looks like this (excluding `open`):
```    
rglob
abspath
isjunction
isdir
stat
lstat
relpath
normpath
is_fifo
mkdir
iterdir
getmtime
islink
hardlink_to
samefile
absolute
walk
is_junction
realpath
is_symlink
getsize
is_socket
getctime
read_bytes
is_file
owner
is_dir
symlink_to
rmdir
sameopenfile
_path_normpath
isfile
chmod
resolve
is_block_device
is_mount
unlink
ismount
rename
glob
write_text
group
lexists
expanduser
readlink
cwd
replace
home
exists
is_char_device
lchmod
open
read_text
touch
getatime
write_bytes
_joinrealpath
```
Length = 57 methods

This is a list of combined async versions of `pathlib.Path` IO methods listed in trio and anyio documentation  + the [deny list from upstream](https://github.com/python-trio/flake8-async/blob/d4c412ecc2ec2d4bdcd3908e0769adf60c1054ed/flake8_async/visitors/visitor2xx.py#L338). I ran this through a Python script to make sure `pathlib.Path` or `os.path` have methods with the same names.

Reversing that and iterating over `pathlib.Path` + `os.path` methods to find non I/O methods (methods that aren't in the deny list and aren't private class methods)  yields about 50 methods, so a "complete" allow list would probably about the same length as the deny list.

---

_Review comment by @PieterCK on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:63 on 2025-09-18 09:50_

How I got the list:
- anyio neatly listed all I/O methods that they support on their website: https://anyio.readthedocs.io/en/latest/api.html#anyio.Path

- for trio, I had to grep it from their website and clean up abit. `grep -oP '(?<=Like ).*(?=, but async)'`. website: https://trio.readthedocs.io/en/stable/reference-io.html

And the python script I used:

<details>

```python
from pathlib import Path

methods_to_check = {
    "absolute",
    "chmod",
    "cwd",
    "exists",
    "expanduser",
    "glob",
    "group",
    "hardlink_to",
    "home",
    "is_block_device",
    "is_char_device",
    "is_dir",
    "is_fifo",
    "is_file",
    "is_junction",
    "is_mount",
    "is_socket",
    "is_symlink",
    "iterdir",
    "lchmod",
    "link_to",
    "lstat",
    "mkdir",
    "open",
    "owner",
    "read_bytes",
    "read_text",
    "readlink",
    "rename",
    "replace",
    "resolve",
    "rglob",
    "rmdir",
    "samefile",
    "stat",
    "symlink_to",
    "touch",
    "unlink",
    "walk",
    "write_bytes",
    "write_text",
}

os_path_methods = {
    "_path_normpath",
    "normpath",
    "_joinrealpath",
    "islink",
    "lexists",
    "ismount",
    "realpath",
    "exists",
    "isdir",
    "isfile",
    "getatime",
    "getctime",
    "getmtime",
    "getsize",
    "samefile",
    "sameopenfile",
    "relpath",
}
import os

path_methods = dir(Path)
deny_list = []
for method in methods_to_check | os_path_methods:
    if method in path_methods or hasattr(os.path, method):
        deny_list.append(method)

print(len(deny_list))
non_blocking_list = []
os_methods = dir(os.path)
for method in set(path_methods) | set(os_methods):
    if method not in deny_list and not method.startswith("_"):
        non_blocking_list.append(method)

print(len(non_blocking_list))
```
</details>

---

_@PieterCK reviewed on 2025-09-18 09:50_

---

_@PieterCK reviewed on 2025-09-18 10:20_

---

_Review comment by @PieterCK on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:63 on 2025-09-18 10:20_

Poking around `os.path` methods that doesn't match the deny list, I found a couple more suspects: `isjunction` and `abspath`.

Both methods look like they perform I/O and probably are `os.path` counterpart for `is_junction` and `absolute` (which are `pathlib` I/O methods). Might miss a few though since I only check suspicious looking ones, so here's the list:

```
['ALLOW_MISSING', 'altsep', 'basename', 'commonpath', 'commonprefix', 'curdir', 'defpath', 'devnull', 'dirname', 'expandvars', 'extsep', 'genericpath', 'isabs', 'join', 'normcase', 'os', 'pardir', 'pathsep', 'samestat', 'sep', 'split', 'splitdrive', 'splitext', 'splitroot', 'supports_unicode_filenames', 'sys']
```

---

_@PieterCK reviewed on 2025-09-18 11:24_

---

_Review comment by @PieterCK on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:59 on 2025-09-18 11:24_

Neat! Implemented this version

---

_@PieterCK reviewed on 2025-09-18 11:24_

---

_Review comment by @PieterCK on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:9 on 2025-09-18 11:24_

Okay, I've added another example using non-blocking methods in async function too.

---

_Review comment by @PieterCK on `crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC240.py`:78 on 2025-09-18 11:26_

Ok! Now `open` passes `maybe_calling_io_operation`. There is a new test for this too


---

_@PieterCK reviewed on 2025-09-18 11:26_

---

_@PieterCK reviewed on 2025-09-18 11:27_

---

_Review comment by @PieterCK on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:144 on 2025-09-18 11:27_

Great! Updated to use `match_initializer` for this check.



---

_@PieterCK reviewed on 2025-09-18 11:29_

---

_Review comment by @PieterCK on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:167 on 2025-09-18 11:29_

Aha, good catch. I moved this a few checks above to just before the constructor check.

---

_@PieterCK reviewed on 2025-09-18 11:31_

---

_Review comment by @PieterCK on `crates/ruff_python_semantic/src/analyze/typing.rs`:929 on 2025-09-18 11:31_

Makes sense, for now I added a copy of `PathlibPathChecker` that has the traversing logic to this rule.

---

_Converted to draft by @PieterCK on 2025-09-18 11:41_

---

_Renamed from "[flake8-async] Implement `blocking-path-method` (ASYNC240)." to "[wip] [flake8-async] Implement `blocking-path-method` (ASYNC240)." by @PieterCK on 2025-09-18 11:41_

---

_Comment by @PieterCK on 2025-09-18 11:42_

Thank you for doing another pass on this! Will post another comment once this is ready for review again.

---

_@ntBre reviewed on 2025-09-18 13:10_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:63 on 2025-09-18 13:10_

Thanks for checking this so thoroughly! I'll defer to you on the allow vs deny list, whichever you think is easier.

---

_@amyreese reviewed on 2025-09-18 15:11_

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:63 on 2025-09-18 15:11_

IMO I think an allowlist is "better" because any new methods added in the future couldn't silently slip by unnoticed by the linter.

---

_@PieterCK reviewed on 2025-09-22 15:28_

---

_Review comment by @PieterCK on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs`:63 on 2025-09-22 15:28_

Ok! Updated it to an allow list

---

_Renamed from "[wip] [flake8-async] Implement `blocking-path-method` (ASYNC240)." to "[flake8-async] Implement `blocking-path-method` (ASYNC240)." by @PieterCK on 2025-09-22 15:34_

---

_Marked ready for review by @PieterCK on 2025-09-22 15:34_

---

_Comment by @PieterCK on 2025-09-22 15:35_

@ntBre -- Marking this as ready for you to take a look again!

---

_@amyreese approved on 2025-09-22 18:21_

looks good to me!

---

_Review comment by @ntBre on `crates/ruff_python_semantic/src/analyze/typing.rs`:919 on 2025-09-22 18:35_

I think it's good to record this TODO somewhere, but I'm kind of leaning toward opening a separate issue and deleting this comment. I feel like we should probably either always traverse unions or never traverse unions and apply that decision to all of our `TypeChecker`s rather than keep a mix of them.

---

_@ntBre approved on 2025-09-22 18:36_

Thank you, this looks good to me too! I just had one comment about moving the todo comment to a separate issue for a follow-up discussion.

---

_Review comment by @PieterCK on `crates/ruff_python_semantic/src/analyze/typing.rs`:919 on 2025-09-23 05:13_

Okay. This commit removes the comment, `Remove comment on PathlibPathChecker.` 

---

_@PieterCK reviewed on 2025-09-23 05:13_

---

_@PieterCK reviewed on 2025-09-23 06:23_

---

_Review comment by @PieterCK on `crates/ruff_python_semantic/src/analyze/typing.rs`:919 on 2025-09-23 06:23_

And opened https://github.com/astral-sh/ruff/issues/20529 to keep track of this. 

---

_@ntBre approved on 2025-09-23 12:30_

Thank you!

---

_Renamed from "[flake8-async] Implement `blocking-path-method` (ASYNC240)." to "[`flake8-async`] Implement `blocking-path-method` (`ASYNC240`)" by @ntBre on 2025-09-23 12:30_

---

_Merged by @ntBre on 2025-09-23 12:30_

---

_Closed by @ntBre on 2025-09-23 12:30_

---

_Comment by @PieterCK on 2025-09-23 14:42_

Awesome! Thank you

---
