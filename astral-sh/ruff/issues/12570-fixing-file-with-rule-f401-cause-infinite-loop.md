```yaml
number: 12570
title: Fixing file with rule F401 cause infinite loop
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2024-07-29T17:38:29Z
updated_at: 2024-07-30T09:54:36Z
url: https://github.com/astral-sh/ruff/issues/12570
synced_at: 2026-01-10T11:09:54Z
```

# Fixing file with rule F401 cause infinite loop

---

_Issue opened by @qarmin on 2024-07-29 17:38_

ruff 0.5.5+353 (2f54d05d9 2024-07-29)
```
ruff check *.py --select F401 --no-cache --fix --unsafe-fixes --preview --output-format concise --isolated
```

file content(at the bottom should be attached raw, not formatted file - github removes some non-printable characters, so copying from here may not work):
```
from .main import MaµToMan
```

error
```
/tmp/tmp_folder/data/5737962126122819962.py:1:8: F401 `tima` imported but unused
Found 501 errors (500 fixed, 1 remaining).
[*] 1 fixable with the --fix option.

debug error: Failed to converge after 500 iterations in `/tmp/tmp_folder/data/5737962126122819962.py` with rule codes F401:---
import timª
---
```

Ruff build, that was used to reproduce problem(compiled on Ubuntu 22.04 with release mode + debug symbols + debug assertions + overflow checks) - https://github.com/qarmin/Automated-Fuzzer/releases/download/Nightly/ruff.7z

[python_compressed.zip](https://github.com/user-attachments/files/16416924/python_compressed.zip)


---

_Label `bug` added by @charliermarsh on 2024-07-29 17:40_

---

_Label `fuzzer` added by @charliermarsh on 2024-07-29 17:40_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-07-29 17:59_

---

_Comment by @AlexWaygood on 2024-07-29 17:59_

Cool bug, thanks!

---

_Comment by @AlexWaygood on 2024-07-29 18:27_

This is an issue with NFKC normalization. If you apply this diff to Ruff:

```diff
diff --git a/crates/ruff_linter/src/fix/codemods.rs b/crates/ruff_linter/src/fix/codemods.rs
index c3c769172..ed4061435 100644
--- a/crates/ruff_linter/src/fix/codemods.rs
+++ b/crates/ruff_linter/src/fix/codemods.rs
@@ -80,7 +80,7 @@ pub(crate) fn remove_imports<'a>(
     for member in member_names {
         let alias_index = aliases
             .iter()
-            .position(|alias| member == qualified_name_from_name_or_attribute(&alias.name));
+            .position(|alias| dbg!(member) == dbg!(qualified_name_from_name_or_attribute(&alias.name)));
         if let Some(index) = alias_index {
             aliases.remove(index);
         }
diff --git a/crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs b/crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs
index bfa884801..4c86744db 100644
--- a/crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs
+++ b/crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs
@@ -397,6 +397,8 @@ pub(crate) fn unused_import(checker: &Checker, scope: &Scope, diagnostics: &mut
         }
         diagnostics.push(diagnostic);
     }
+
+    panic!()
 }
```

Then the output from the debug calls is as follows:

```
[crates/ruff_linter/src/fix/codemods.rs:83:31] member = "MaμToMan"
[crates/ruff_linter/src/fix/codemods.rs:83:47] qualified_name_from_name_or_attribute(&alias.name) = "MaµToMan"
```

And I'll also paste a screenshot because it looks like GitHub might actually normalize these confusables as well when it renders them in an issue comment?!):

---

![image](https://github.com/user-attachments/assets/6a4a30c8-93d3-418b-a906-085f1484b0e4)

---

If you look _very_ closely at that screenshot, you can see that the `μ` on the first line is sliiiightly narrower in size than the `µ` on the second line. In other words, one of the two variables has been NFKC-normalized while the other has not, meaning the comparison here evaluates to `false` when it should be evaluating to `true`:

https://github.com/astral-sh/ruff/blob/381bd1ff4a38e0582618e76ae1bd3696b1b2ff5d/crates/ruff_linter/src/fix/codemods.rs#L83

---

_Closed by @AlexWaygood on 2024-07-30 09:54_

---
