```yaml
number: 15853
title: "[`flake8-pyi`] Fix incorrect behaviour of `custom-typevar-return-type` preview-mode autofix if `typing` was already imported (`PYI019`)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - fixes
  - preview
assignees: []
merged: true
base: main
head: alex/pyi-self-binding-2
created_at: 2025-01-31T15:00:14Z
updated_at: 2025-01-31T16:46:33Z
url: https://github.com/astral-sh/ruff/pull/15853
synced_at: 2026-01-12T15:55:52Z
```

# [`flake8-pyi`] Fix incorrect behaviour of `custom-typevar-return-type` preview-mode autofix if `typing` was already imported (`PYI019`)

---

_@AlexWaygood_

## Summary

`Importer::get_or_import_symbol` can fail, and if it does it returns a useful `ResolutionError` that tells you why it failed. The message from this error is printed to the terminal if you run Ruff with enough `-v` arguments on the command line. However, we're currently throwing away this useful information in the implementation of this rule by calling `.ok()` on the value returned by `get_or_import_symbol`. There's no reason to do this, as it doesn't really simplify the code; we already have a `Diagnostic::try_set_optional_fix()` method that's designed exactly for this situation.

The fix also makes an incorrect edit if `typing` is already imported in the scope being edited -- here, it makes a fix that would introduce a `NameError` (because `Self` is not imported anywhere in the file):

````
~/dev/ruff (main)⚡ [2] % cargo run -- check --no-cache --preview --diff --fix --select=PYI019 --target-version=py313 foo.pyi
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.11s
     Running `target/debug/ruff check --no-cache --preview --diff --fix --select=PYI019 --target-version=py313 foo.pyi`
--- foo.pyi
+++ foo.pyi
@@ -1,5 +1,5 @@
 import typing
 
 class F:
-    def m[S](self: S) -> S: ...
+    def m(self) -> Self: ...
````

This is because we throw away the second element of the tuple returned by `get_or_import_symbol_here`:

https://github.com/astral-sh/ruff/blob/f1418be81ca4dc3a198d06095b75c83d21088461/crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs#L335-L337

In this case, the second element would be the string `"typing.Self"`, which is what we should use in the `Edit`. But instead we just pass in the string `"Self"`, assuming the at the import edit would have added `from typing import Self` at the top of the module (it only does that if both `typing` nor `Self` are currently not imported):

https://github.com/astral-sh/ruff/blob/f1418be81ca4dc3a198d06095b75c83d21088461/crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs#L354-L356 

## Test Plan

I added some fixtures that exhibit incorrect behaviour on `main`.


---

_Label `bug` added by @AlexWaygood on 2025-01-31 15:00_

---

_Label `fixes` added by @AlexWaygood on 2025-01-31 15:00_

---

_Label `preview` added by @AlexWaygood on 2025-01-31 15:01_

---

_Renamed from "[`flake8-pyi`] Fix incorrect behaviour of `custom-typevar-return-type` autofix if `typing` was already imported (`PYI019`)" to "[`flake8-pyi`] Fix incorrect behaviour of `custom-typevar-return-type` preview-mode autofix if `typing` was already imported (`PYI019`)" by @AlexWaygood on 2025-01-31 15:01_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI019_PYI019_1.pyi.snap`:1 on 2025-01-31 15:01_

This is the snapshot for if preview mode is not enabled (the autofix is only availabe with `--preview` currently)

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__preview_PYI019_PYI019_1.pyi.snap`:1 on 2025-01-31 15:02_

this is the snapshot for if `--preview` mode is enabled

---

_@AlexWaygood reviewed on 2025-01-31 15:02_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_return_type.rs`:278 on 2025-01-31 15:03_

I moved the check for whether or not it's a stub file inside the body of `replace_custom_typevar_with_self()`, since that function already returns an `Option`. This means we don't have to have comments in two places :-)

---

_@AlexWaygood reviewed on 2025-01-31 15:03_

---

_Review requested from @ntBre by @AlexWaygood on 2025-01-31 15:04_

---

_Review requested from @dylwil3 by @AlexWaygood on 2025-01-31 15:04_

---

_Comment by @github-actions[bot] on 2025-01-31 15:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-01-31 16:45_

This looks great!

---

_Merged by @AlexWaygood on 2025-01-31 16:46_

---

_Closed by @AlexWaygood on 2025-01-31 16:46_

---

_Branch deleted on 2025-01-31 16:46_

---
