```yaml
number: 18946
title: "Convert `OldDiagnostic::noqa_code` to an `Option<String>`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: brent/string-noqa-code
created_at: 2025-06-25T21:19:40Z
updated_at: 2025-06-27T15:36:57Z
url: https://github.com/astral-sh/ruff/pull/18946
synced_at: 2026-01-12T15:56:28Z
```

# Convert `OldDiagnostic::noqa_code` to an `Option<String>`

---

_@ntBre_

## Summary

I think this should be the last step before combining `OldDiagnostic` and `ruff_db::Diagnostic`. We can't store a `NoqaCode` on `ruff_db::Diagnostic`, so I converted the `noqa_code` field to an `Option<String>` and then propagated this change to all of the callers. 

I tried to use `&str` everywhere it was possible, so I think the remaining `to_string` calls are necessary. I spent some time trying to convert _everything_ to `&str` but ran into lifetime issues, especially in the `FixTable`. Maybe we can take another look at that if it causes a performance regression, but hopefully these paths aren't too hot. We also avoid some `to_string` calls, so it might even out a bit too.

## Test Plan

Existing tests


---

_Label `internal` added by @ntBre on 2025-06-25 21:19_

---

_Label `diagnostics` added by @ntBre on 2025-06-25 21:19_

---

_Comment by @github-actions[bot] on 2025-06-25 21:29_

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

_Comment by @ntBre on 2025-06-25 21:38_

The performance is a bit worse (-2% in the worst case) on the all-rules benchmarks, but it seems widely dispersed, so I think it may just be from the up-front `NoqaCode::to_string` calls for storing them in `OldDiagnostic`, not the later conversions.

---

_Marked ready for review by @ntBre on 2025-06-25 21:38_

---

_Review requested from @MichaReiser by @ntBre on 2025-06-25 21:38_

---

_@MichaReiser reviewed on 2025-06-26 07:10_

---

_Review comment by @MichaReiser on `crates/ruff/src/diagnostics.rs`:170 on 2025-06-26 07:10_

We could use hashbrown with `entry_ref` here to avoid allocating a new string only to increment the counter

<details><summary>Patch<summary>
<p>

```patch
Index: Cargo.lock
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/Cargo.lock b/Cargo.lock
--- a/Cargo.lock	(revision 44c13a29c5f24064c446277cfdfd9fdba6c163f6)
+++ b/Cargo.lock	(date 1750921570209)
@@ -2812,6 +2812,7 @@
  "fern",
  "glob",
  "globset",
+ "hashbrown 0.15.4",
  "imperative",
  "insta",
  "is-macro",
Index: crates/ruff_linter/src/linter.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_linter/src/linter.rs b/crates/ruff_linter/src/linter.rs
--- a/crates/ruff_linter/src/linter.rs	(revision 44c13a29c5f24064c446277cfdfd9fdba6c163f6)
+++ b/crates/ruff_linter/src/linter.rs	(date 1750921780496)
@@ -1,12 +1,11 @@
 use std::borrow::Cow;
-use std::collections::hash_map::Entry;
 use std::path::Path;
 
 use anyhow::{Result, anyhow};
 use colored::Colorize;
 use itertools::Itertools;
 use ruff_python_parser::semantic_errors::SemanticSyntaxError;
-use rustc_hash::FxHashMap;
+use rustc_hash::FxBuildHasher;
 
 use ruff_notebook::Notebook;
 use ruff_python_ast::{ModModule, PySourceType, PythonVersion};
@@ -94,15 +93,15 @@
 
 /// A mapping from a noqa code to the corresponding lint name and a count of applied fixes.
 #[derive(Debug, Default, PartialEq)]
-pub struct FixTable(FxHashMap<String, FixCount>);
+pub struct FixTable(hashbrown::HashMap<String, FixCount, rustc_hash::FxBuildHasher>);
 
 impl FixTable {
     pub fn counts(&self) -> impl Iterator<Item = usize> {
         self.0.values().map(|fc| fc.count)
     }
 
-    pub fn entry(&mut self, code: String) -> FixTableEntry {
-        FixTableEntry(self.0.entry(code))
+    pub fn entry<'a>(&'a mut self, code: &'a str) -> FixTableEntry<'a> {
+        FixTableEntry(self.0.entry_ref(code))
     }
 
     pub fn iter(&self) -> impl Iterator<Item = (&str, &'static str, usize)> {
@@ -120,7 +119,9 @@
     }
 }
 
-pub struct FixTableEntry<'a>(Entry<'a, String, FixCount>);
+pub struct FixTableEntry<'a>(
+    hashbrown::hash_map::EntryRef<'a, 'a, String, str, FixCount, FxBuildHasher>,
+);
 
 impl<'a> FixTableEntry<'a> {
     pub fn or_default(self, rule_name: &'static str) -> &'a mut usize {
@@ -650,7 +651,7 @@
             if iterations < MAX_ITERATIONS {
                 // Count the number of fixed errors.
                 for (rule, name, count) in applied.iter() {
-                    *fixed.entry(rule.to_string()).or_default(name) += count;
+                    *fixed.entry(rule).or_default(name) += count;
                 }
 
                 transformed = Cow::Owned(transformed.updated(fixed_contents, &source_map));
Index: crates/ruff/src/diagnostics.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff/src/diagnostics.rs b/crates/ruff/src/diagnostics.rs
--- a/crates/ruff/src/diagnostics.rs	(revision 44c13a29c5f24064c446277cfdfd9fdba6c163f6)
+++ b/crates/ruff/src/diagnostics.rs	(date 1750921482563)
@@ -167,7 +167,7 @@
             let fixed_in_file = self.0.entry(filename).or_default();
             for (rule, name, count) in fixed.iter() {
                 if count > 0 {
-                    *fixed_in_file.entry(rule.to_string()).or_default(name) += count;
+                    *fixed_in_file.entry(rule).or_default(name) += count;
                 }
             }
         }
Index: crates/ruff_linter/Cargo.toml
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_linter/Cargo.toml b/crates/ruff_linter/Cargo.toml
--- a/crates/ruff_linter/Cargo.toml	(revision 44c13a29c5f24064c446277cfdfd9fdba6c163f6)
+++ b/crates/ruff_linter/Cargo.toml	(date 1750921569921)
@@ -38,6 +38,7 @@
 fern = { workspace = true }
 glob = { workspace = true }
 globset = { workspace = true }
+hashbrown = { workspace = true }
 imperative = { workspace = true }
 is-macro = { workspace = true }
 is-wsl = { workspace = true }
Index: crates/ruff_linter/src/fix/mod.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_linter/src/fix/mod.rs b/crates/ruff_linter/src/fix/mod.rs
--- a/crates/ruff_linter/src/fix/mod.rs	(revision 44c13a29c5f24064c446277cfdfd9fdba6c163f6)
+++ b/crates/ruff_linter/src/fix/mod.rs	(date 1750921753842)
@@ -110,7 +110,7 @@
         }
 
         applied.extend(applied_edits.drain(..));
-        *fixed.entry(code.to_string()).or_default(name) += 1;
+        *fixed.entry(code).or_default(name) += 1;
     }
 
     // Add the remaining content.

```

</p>
</details> 

---

_Review comment by @MichaReiser on `crates/ruff/src/printer.rs`:309 on 2025-06-26 07:11_

I wonder if we should add a `SecondaryCode` struct which wraps a `String` I do find the `&str` makes the code harder to understand.

---

_Review comment by @MichaReiser on `crates/ruff/src/printer.rs`:365 on 2025-06-26 07:12_

```suggestion
                        statistic.code.as_ref().unwrap_or_default().red().bold(),
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/noqa.rs`:54 on 2025-06-26 07:13_

```suggestion
        if code == Rule::BlanketNOQA.noqa_code() {
```

We should add another `PartialEq` override if that's the only reason for changing the order here

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/noqa.rs`:81 on 2025-06-26 07:14_

Could `matches` now be a `Vec<&'a str>` instead?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/mod.rs`:64 on 2025-06-26 07:42_

We should rename this field to match the function name

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/text.rs`:255 on 2025-06-26 07:43_

```suggestion
        let label = self.message.secondary_code().unwrap_or_default();
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:210 on 2025-06-26 08:06_

I'm inclined to keep using `Rule` here:

* We anyway parse the rule code in the `Directive::Codes` branch
* I think we want to migrate to a model similar to ty long term where we collect all suppression comments and then check them inside `report_diagnostic` (where we have access to the `Rule`!)



---

_@MichaReiser reviewed on 2025-06-26 08:08_

This mostly looks good. I think we want to keep using `Rule` in `noqa.rs`. But I'm not sure if that's better done in a separate PR

I also suggest introducing a `SecondaryCode` struct. It's hard to tell what all the `&str`  and `String` usages are (especially because there are so few comments)

---

_@MichaReiser approved on 2025-06-26 08:08_

---

_Review comment by @ntBre on `crates/ruff_linter/src/noqa.rs`:210 on 2025-06-26 12:49_

`Rule` instead of reverting to `NoqaCode`? I don't think we always parse this back to a `Rule`, but I can give it a try if you want. This commit should show roughly all the places that could be affected: https://github.com/astral-sh/ruff/pull/18946/commits/97607a524cde36563a83ad1f10dbec35863625d2.

---

_@ntBre reviewed on 2025-06-26 12:49_

---

_Review comment by @ntBre on `crates/ruff/src/printer.rs`:309 on 2025-06-26 12:51_

Hmm, I think if it wrapped a `String` we'd have to clone in `OldDiagnostic::secondary_code` right? I'll try it with a `&str` and see if we can use that everywhere. I definitely agree that it would be more readable, though.

---

_@ntBre reviewed on 2025-06-26 12:51_

---

_@ntBre reviewed on 2025-06-26 13:03_

---

_Review comment by @ntBre on `crates/ruff/src/diagnostics.rs`:170 on 2025-06-26 13:03_

Nice, thanks for the patch! It was annoying that I couldn't do this with a regular `Entry`.

---

_@MichaReiser reviewed on 2025-06-26 13:19_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:210 on 2025-06-26 13:19_

Yes, I'm inclined to use `Rule` here instead of the secondary code.

---

_@ntBre reviewed on 2025-06-26 13:21_

---

_Review comment by @ntBre on `crates/ruff_linter/src/noqa.rs`:210 on 2025-06-26 13:21_

Sounds good, I see what you mean now about using this in `report_diagnostic` eventually, so `Rule` is a step in that direction. Thanks!

---

_@ntBre reviewed on 2025-06-26 13:30_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/noqa.rs`:81 on 2025-06-26 13:30_

I don't think so, but this is where I ran into lifetime issues. First in `check_noqa`, where iterating over the context creates a mutable borrow, but we need an immutable one later on.

https://github.com/astral-sh/ruff/blob/d04e63a6d9dbb5c751c99d113d7eaf98b765b426/crates/ruff_linter/src/checkers/noqa.rs#L47-L49

We can possibly get around that by doing the iteration after the immutable borrows, but I'm not sure if that breaks other logic. It at least satisfies the compiler so that I can hit the next error.

And then in `FileNoqaDirectives::extract` where the compiler says I'm returning a local reference.

https://github.com/astral-sh/ruff/blob/d04e63a6d9dbb5c751c99d113d7eaf98b765b426/crates/ruff_linter/src/noqa.rs#L263-L266

Do you see anything obviously wrong I'm doing here?

I tried somewhat briefly to move this noqa logic into `LintContext` a couple of days ago, like we did with the per-file ignores, but it turned out to be trickier than expected and I postponed it to work on this PR.

---

_@MichaReiser reviewed on 2025-06-26 14:35_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:210 on 2025-06-26 14:35_

Feel free to merge this PR and do this refactor in a separate PR (or maybe the best is if you try the change in a separate PR and revert your changes in this PR if it works out)

---

_@ntBre reviewed on 2025-06-26 14:41_

---

_Review comment by @ntBre on `crates/ruff/src/printer.rs`:309 on 2025-06-26 14:41_

I think I was responding to these too early in the morning :facepalm: I can store the `SecondaryCode(String)` on the `OldDiagnostic` and then use `&SecondaryCode` where I'm currently using `&str`. I'm working on that, and then I'll try the `Rule` changes on a separate branch!

---

_@MichaReiser reviewed on 2025-06-26 14:53_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/noqa.rs`:81 on 2025-06-26 14:53_

No, I think the solution here is to use `Rule`

---

_Comment by @github-actions[bot] on 2025-06-26 16:40_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@ntBre reviewed on 2025-06-26 17:07_

---

_Review comment by @ntBre on `crates/ruff_linter/src/noqa.rs`:210 on 2025-06-26 17:07_

I opened a small PR making this change: https://github.com/astral-sh/ruff/pull/18966. Hopefully that's what you had in mind.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/mod.rs`:390 on 2025-06-27 06:27_

```suggestion
#[derive(Clone, Debug, PartialEq, Eq, Default, Hash)]
```

Or do we use the ordering somewhere? 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/mod.rs`:448 on 2025-06-27 06:27_

YOu can derive this with `serde(transparent)`

---

_@MichaReiser approved on 2025-06-27 06:28_

---

_@ntBre reviewed on 2025-06-27 12:42_

---

_Review comment by @ntBre on `crates/ruff_linter/src/message/mod.rs`:390 on 2025-06-27 12:42_

I think they're used for adding noqa codes, but I'll double check.

---

_@ntBre reviewed on 2025-06-27 12:48_

---

_Review comment by @ntBre on `crates/ruff_linter/src/message/mod.rs`:390 on 2025-06-27 12:48_

Yeah, they're used in `collect_rule_codes`:

https://github.com/astral-sh/ruff/blob/b1f4e536291d75018ee14062050b3ff89eb66d46/crates/ruff_linter/src/linter.rs#L682-L687

and in `NoqaEdit::write`. We could map to strings first if that would be preferable over deriving these, though.

---

_@MichaReiser reviewed on 2025-06-27 14:15_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/mod.rs`:390 on 2025-06-27 14:15_

Nah, it's fine then. 

---

_Merged by @ntBre on 2025-06-27 15:36_

---

_Closed by @ntBre on 2025-06-27 15:36_

---

_Branch deleted on 2025-06-27 15:36_

---
