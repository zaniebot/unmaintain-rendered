```yaml
number: 20106
title: "[`pylint`] Add U+061C to `PLE2502`"
type: pull_request
state: merged
author: kotx
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: main
created_at: 2025-08-26T21:52:05Z
updated_at: 2025-08-28T20:35:49Z
url: https://github.com/astral-sh/ruff/pull/20106
synced_at: 2026-01-10T17:46:21Z
```

# [`pylint`] Add U+061C to `PLE2502`

---

_Pull request opened by @kotx on 2025-08-26 21:52_

Resolves #20058


---

_@ntBre reviewed on 2025-08-27 18:54_

Thank you!

I'm a bit conflicted because this clearly seems within the original intent of the rule, but a conservative reading of our [versioning guidelines](https://docs.astral.sh/ruff/versioning/#version-changes) suggests that this needs to be a preview change since it increases the scope of a stable rule.

Would you mind making this a preview change? You can find examples of our preview checks in this file:

https://github.com/astral-sh/ruff/blob/e5e217fcd611ab24f516b2af4fa21e25eadc5645/crates/ruff_linter/src/preview.rs#L254-L257

and I'd recommend making a copy of the test function here with preview enabled (often called `preview_rules`):

https://github.com/astral-sh/ruff/blob/e5e217fcd611ab24f516b2af4fa21e25eadc5645/crates/ruff_linter/src/rules/pylint/mod.rs#L237

with just one `test_case` for `bidirectional_unicode.py`.

We'll probably want to move the character out of `BIDI_UNICODE` for now and combine it with the preview check, but what you have here is perfect once we stabilize the behavior.

---

_Label `rule` added by @ntBre on 2025-08-27 18:55_

---

_Label `preview` added by @ntBre on 2025-08-27 18:55_

---

_Comment by @github-actions[bot] on 2025-08-27 19:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @kotx on 2025-08-27 19:43_

@ntBre The guidelines say minor version is bumped when "The scope of a stable rule is *significantly* increased" (emphasis added). Is this significant enough to warrant a preview change? Will proceed if this is the case (thank you for the detailed instructions!).

---

_Comment by @ntBre on 2025-08-27 21:06_

Yeah, that's why I was conflicted. I guess I consider it significant within the niche of the rule itself, and it just errs on the safe side to keep it in preview. It's also a departure from the upstream `pylint` rule based on a quick test I just did, which is another reason.

---

_Review requested from @ntBre by @kotx on 2025-08-28 02:02_

---

_Comment by @ntBre on 2025-08-28 15:48_

Thank you! This looks good overall. I did try to push one small patch to put the new test case back in `bidirectional_unicode.py` instead of a separate `_preview` file. That will make it a little easier to update things when we stabilize the change. But I couldn't push to the PR branch for some reason. Would you mind applying this patch or making similar changes? This is good to go otherwise.

<details><summary>Patch</summary>
<p>

```diff
diff --git a/crates/ruff_linter/resources/test/fixtures/pylint/bidirectional_unicode.py b/crates/ruff_linter/resources/test/fixtures/pylint/bidirectional_unicode.py
index 25accb3aad..f72695d0c4 100644
--- a/crates/ruff_linter/resources/test/fixtures/pylint/bidirectional_unicode.py
+++ b/crates/ruff_linter/resources/test/fixtures/pylint/bidirectional_unicode.py
@@ -4,6 +4,9 @@ print("שלום‬")
 # E2502
 example = "x‏" * 100  #    "‏x" is assigned
 
+# E2502
+another = "x؜" * 50  #    "؜x" is assigned
+
 # E2502
 if access_level != "none‮⁦":  # Check if admin ⁩⁦' and access_level != 'user
     print("You are an admin.")
diff --git a/crates/ruff_linter/resources/test/fixtures/pylint/bidirectional_unicode_preview.py b/crates/ruff_linter/resources/test/fixtures/pylint/bidirectional_unicode_preview.py
deleted file mode 100644
index 1445423c99..0000000000
--- a/crates/ruff_linter/resources/test/fixtures/pylint/bidirectional_unicode_preview.py
+++ /dev/null
@@ -1,11 +0,0 @@
-# E2502
-hello = "x؜" * 50  #    "؜x" is assigned
-
-# OK
-print("\u061C")
-
-# OK
-print("\N{ARABIC LETTER MARK}")
-
-# OK
-print("Hello World")
diff --git a/crates/ruff_linter/src/rules/pylint/mod.rs b/crates/ruff_linter/src/rules/pylint/mod.rs
index 9d2d26072e..e181c0a146 100644
--- a/crates/ruff_linter/src/rules/pylint/mod.rs
+++ b/crates/ruff_linter/src/rules/pylint/mod.rs
@@ -252,12 +252,13 @@ mod tests {
         Ok(())
     }
 
-    #[test_case(
-        Rule::BidirectionalUnicode,
-        Path::new("bidirectional_unicode_preview.py")
-    )]
+    #[test_case(Rule::BidirectionalUnicode, Path::new("bidirectional_unicode.py"))]
     fn preview_rules(rule_code: Rule, path: &Path) -> Result<()> {
-        let snapshot = format!("{}_{}", rule_code.noqa_code(), path.to_string_lossy());
+        let snapshot = format!(
+            "preview__{}_{}",
+            rule_code.noqa_code(),
+            path.to_string_lossy()
+        );
         let diagnostics = test_path(
             Path::new("pylint").join(path).as_path(),
             &LinterSettings {
diff --git a/crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLE2502_bidirectional_unicode.py.snap b/crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLE2502_bidirectional_unicode.py.snap
index 18356831d7..0d584e8108 100644
--- a/crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLE2502_bidirectional_unicode.py.snap
+++ b/crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLE2502_bidirectional_unicode.py.snap
@@ -22,21 +22,21 @@ PLE2502 Contains control characters that can permit obfuscated code
   |
 
 PLE2502 Contains control characters that can permit obfuscated code
- --> bidirectional_unicode.py:8:1
-  |
-7 | # E2502
-8 | if access_level != "none":  # Check if admin ' and access_level != 'user
-  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
-9 |     print("You are an admin.")
-  |
+  --> bidirectional_unicode.py:11:1
+   |
+10 | # E2502
+11 | if access_level != "none":  # Check if admin ' and access_level != 'user
+   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
+12 |     print("You are an admin.")
+   |
 
 PLE2502 Contains control characters that can permit obfuscated code
-  --> bidirectional_unicode.py:14:1
+  --> bidirectional_unicode.py:17:1
    |
-12 | # E2502
-13 | def subtract_funds(account: str, amount: int):
-14 |     """Subtract funds from bank account then """
+15 | # E2502
+16 | def subtract_funds(account: str, amount: int):
+17 |     """Subtract funds from bank account then """
    | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
-15 |     return
-16 |     bank[account] -= amount
+18 |     return
+19 |     bank[account] -= amount
    |
diff --git a/crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__preview__PLE2502_bidirectional_unicode.py.snap b/crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__preview__PLE2502_bidirectional_unicode.py.snap
new file mode 100644
index 0000000000..4a1555ebe8
--- /dev/null
+++ b/crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__preview__PLE2502_bidirectional_unicode.py.snap
@@ -0,0 +1,52 @@
+---
+source: crates/ruff_linter/src/rules/pylint/mod.rs
+---
+PLE2502 Contains control characters that can permit obfuscated code
+ --> bidirectional_unicode.py:2:1
+  |
+1 | # E2502
+2 | print("שלום")
+  | ^^^^^^^^^^^^^
+3 |
+4 | # E2502
+  |
+
+PLE2502 Contains control characters that can permit obfuscated code
+ --> bidirectional_unicode.py:5:1
+  |
+4 | # E2502
+5 | example = "x‏" * 100  #    "‏x" is assigned
+  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
+6 |
+7 | # E2502
+  |
+
+PLE2502 Contains control characters that can permit obfuscated code
+  --> bidirectional_unicode.py:8:1
+   |
+ 7 | # E2502
+ 8 | another = "x؜" * 50  #    "؜x" is assigned
+   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
+ 9 |
+10 | # E2502
+   |
+
+PLE2502 Contains control characters that can permit obfuscated code
+  --> bidirectional_unicode.py:11:1
+   |
+10 | # E2502
+11 | if access_level != "none":  # Check if admin ' and access_level != 'user
+   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
+12 |     print("You are an admin.")
+   |
+
+PLE2502 Contains control characters that can permit obfuscated code
+  --> bidirectional_unicode.py:17:1
+   |
+15 | # E2502
+16 | def subtract_funds(account: str, amount: int):
+17 |     """Subtract funds from bank account then """
+   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
+18 |     return
+19 |     bank[account] -= amount
+   |
```

</p>
</details> 

This is mostly restoring the test case you had before, which is the only reason I was going to push it quickly and then merge instead of bothering you again :)

---

_Comment by @kotx on 2025-08-28 19:01_

@ntBre pushed!

---

_@ntBre reviewed on 2025-08-28 19:34_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLE2502_bidirectional_unicode_preview.py.snap`:1 on 2025-08-28 19:34_

I think you might have to delete this now-unused snapshot to pass CI

---

_@kotx reviewed on 2025-08-28 19:42_

---

_Review comment by @kotx on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLE2502_bidirectional_unicode_preview.py.snap`:1 on 2025-08-28 19:42_

Fixed!

---

_Comment by @ntBre on 2025-08-28 20:01_

Did you fix the clippy [error](https://github.com/astral-sh/ruff/actions/runs/17305337828/job/49128933685) too? I think it just wanted you to combine your if branches with an `||`. I noticed that but wasn't going to comment since I think it's easier to read this way, but clippy wasn't so generous 😆 

---

_Comment by @kotx on 2025-08-28 20:14_

You're right, I had totally missed that! Fixed

---

_@ntBre approved on 2025-08-28 20:31_

Thanks!

---

_Renamed from "Add U+061C to PLE2502" to "[`pylint`] Add U+061C to `PLE2502`" by @ntBre on 2025-08-28 20:35_

---

_Merged by @ntBre on 2025-08-28 20:35_

---

_Closed by @ntBre on 2025-08-28 20:35_

---
