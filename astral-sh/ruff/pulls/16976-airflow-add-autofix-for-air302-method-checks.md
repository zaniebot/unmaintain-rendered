```yaml
number: 16976
title: "[`airflow`] Add autofix for `AIR302` method checks"
type: pull_request
state: merged
author: Lee-W
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: autofix-AIR302-check-method
created_at: 2025-03-26T08:09:12Z
updated_at: 2025-04-02T15:05:49Z
url: https://github.com/astral-sh/ruff/pull/16976
synced_at: 2026-01-12T15:55:59Z
```

# [`airflow`] Add autofix for `AIR302` method checks

---

_@Lee-W_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Add autofix logic to AIR302 check_method

## Test Plan

<!-- How was it tested? -->

test fixtures have been updated accordingly


---

_Comment by @Lee-W on 2025-03-26 08:09_

based on https://github.com/astral-sh/ruff/pull/16975

---

_Comment by @github-actions[bot] on 2025-03-26 08:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select A,E703,F704,B015,B018,D100</pre>
</p>
<p>

```
Failed to clone openai/openai-cookbook: error: 4282 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @Lee-W on 2025-03-27 19:35_

---

_@dhruvmanila reviewed on 2025-03-31 20:59_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:511 on 2025-03-31 20:59_

We can avoid the clone here by creating the `Fix`, before creating the `Diagnostic`.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:511 on 2025-03-31 21:02_

Here's the patch as I can't push it to the branch:

```diff
diff --git a/crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs b/crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs
index 2111b60b3..fc9f40551 100644
--- a/crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs
+++ b/crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs
@@ -72,7 +72,7 @@ impl Violation for Airflow3Removal {
     }
 }
 
-#[derive(Debug, Clone, Eq, PartialEq)]
+#[derive(Debug, Eq, PartialEq)]
 enum Replacement {
     None,
     Name(&'static str),
@@ -505,18 +505,25 @@ fn check_method(checker: &Checker, call_expr: &ExprCall) {
         }
     };
 
+    // Create the `Fix` first to avoid cloning `Replacement`.
+    let fix = if let Replacement::Name(name) = replacement {
+        Some(Fix::safe_edit(Edit::range_replacement(
+            name.to_string(),
+            attr.range(),
+        )))
+    } else {
+        None
+    };
+
     let mut diagnostic = Diagnostic::new(
         Airflow3Removal {
             deprecated: attr.to_string(),
-            replacement: replacement.clone(),
+            replacement,
         },
         attr.range(),
     );
-    if let Replacement::Name(name) = replacement {
-        diagnostic.set_fix(Fix::safe_edit(Edit::range_replacement(
-            name.to_string(),
-            attr.range(),
-        )));
+    if let Some(fix) = fix {
+        diagnostic.set_fix(fix);
     }
     checker.report_diagnostic(diagnostic);
 }
```

---

_@dhruvmanila approved on 2025-03-31 21:02_

---

_Label `fixes` added by @dhruvmanila on 2025-03-31 21:03_

---

_Label `preview` added by @dhruvmanila on 2025-03-31 21:03_

---

_Renamed from "[airflow] add autofix to AIR302 check_method" to "[`airflow`] Add autofix for `AIR302` method checks" by @dhruvmanila on 2025-03-31 21:03_

---

_@Lee-W reviewed on 2025-04-01 07:28_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:511 on 2025-04-01 07:28_

just updated. Thanks!

---

_Merged by @dhruvmanila on 2025-04-02 15:05_

---

_Closed by @dhruvmanila on 2025-04-02 15:05_

---
