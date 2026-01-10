```yaml
number: 19413
title: "[`pyupgrade`] Prevent infinite loop with `I002` (`UP010`, `UP035`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-18729
created_at: 2025-07-17T21:04:05Z
updated_at: 2025-07-31T20:44:42Z
url: https://github.com/astral-sh/ruff/pull/19413
synced_at: 2026-01-10T17:52:17Z
```

# [`pyupgrade`] Prevent infinite loop with `I002` (`UP010`, `UP035`)

---

_Pull request opened by @danparizher on 2025-07-17 21:04_

## Summary

Fixes #18729 and fixes #16802

## Test Plan

Manually verified via CLI that Ruff no longer enters an infinite loop by running:
```sh
echo 1 | ruff --isolated check - --select I002,UP010 --fix
```
with `required-imports = ["from __future__ import generator_stop"]` set in the config, confirming “All checks passed!” and no snapshots were generated.


---

_Comment by @github-actions[bot] on 2025-07-17 21:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @ntBre on 2025-07-24 13:08_

---

_Label `bug` added by @ntBre on 2025-07-25 22:02_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_future_import.rs`:129 on 2025-07-28 18:44_

This looks like a nice potential helper function. Refactoring it like that would both allow us to reuse some code here with an early return and share this with UP035, which I think could use the same fix (https://github.com/astral-sh/ruff/issues/16802). I'd suggest something like:

```rust
fn helper_function(/* ... */) -> bool {
  let name_import = match stmt {
              ruff_python_ast::Stmt::ImportFrom(ruff_python_ast::StmtImportFrom {
                  module,
                  level,
                  ..
              }) => {
                  NameImport::ImportFrom(MemberNameImport {
                      module: module.as_ref().map(std::string::ToString::to_string),
                      name: ruff_python_semantic::Alias {
                          name: alias.name.to_string(),
                          as_name: None,
                      },
                      level: *level,
                  })
              }
              ruff_python_ast::Stmt::Import(_) => {
                  NameImport::Import(ModuleNameImport {
                      name: ruff_python_semantic::Alias {
                          name: alias.name.to_string(),
                          as_name: None,
                      },
                  })

              }
              _ => return false,
          };
                  checker
                      .settings()
                      .isort
                      .required_imports
                      .contains(&name_import)
}
```

modulo the horrific formatting from me editing this on GitHub and with a real function name and arguments.

---

_@ntBre reviewed on 2025-07-28 18:46_

Thanks! I had one suggestion to refactor the fix here, and we should also add a test. Since the fix is on the UP010 side, I think it would make sense to add a unit test in the `pyupgrade` module too. You should be able to select both rules and use the normal snapshot infrastructure otherwise.

---

_Comment by @danparizher on 2025-07-28 23:39_

Thanks, I extracted that as a function and also added a check into `crates\ruff_linter\src\rules\pyupgrade\rules\deprecated_import.rs`. Let me know if that's correct. For adding tests for the whole `pyupgrade` module, what would that look like? Would appreciate recommendations

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_future_import.rs`:88 on 2025-07-29 17:56_

I think we could pass a `StmtRef` here and avoid the clone in UP035.

---

_@ntBre reviewed on 2025-07-29 18:18_

I found one new suggestion for the implementation, but this looks good otherwise.

For the test, I wrote up this patch:

<details><summary>Patch</summary>

```diff
diff --git a/crates/ruff_linter/src/rules/pyupgrade/mod.rs b/crates/ruff_linter/src/rules/pyupgrade/mod.rs
index 3f64cb323f..919958e48e 100644
--- a/crates/ruff_linter/src/rules/pyupgrade/mod.rs
+++ b/crates/ruff_linter/src/rules/pyupgrade/mod.rs
@@ -7,16 +7,18 @@ pub(crate) mod types;
 
 #[cfg(test)]
 mod tests {
+    use std::collections::BTreeSet;
     use std::path::Path;
 
     use anyhow::Result;
     use ruff_python_ast::PythonVersion;
+    use ruff_python_semantic::{MemberNameImport, NameImport};
     use test_case::test_case;
 
     use crate::registry::Rule;
-    use crate::rules::pyupgrade;
+    use crate::rules::{isort, pyupgrade};
     use crate::settings::types::PreviewMode;
-    use crate::test::test_path;
+    use crate::test::{test_path, test_snippet};
     use crate::{assert_diagnostics, settings};
 
     #[test_case(Rule::ConvertNamedTupleFunctionalToClass, Path::new("UP014.py"))]
@@ -294,4 +296,44 @@ mod tests {
         assert_diagnostics!(diagnostics);
         Ok(())
     }
+
+    #[test]
+    fn i002_conflict() {
+        let diagnostics = test_snippet(
+            "1",
+            &settings::LinterSettings {
+                isort: isort::settings::Settings {
+                    required_imports: BTreeSet::from_iter([
+                        // https://github.com/astral-sh/ruff/issues/18729
+                        NameImport::ImportFrom(MemberNameImport::member(
+                            "__future__".to_string(),
+                            "generator_stop".to_string(),
+                        )),
+                        // https://github.com/astral-sh/ruff/issues/16802
+                        NameImport::ImportFrom(MemberNameImport::member(
+                            "collections".to_string(),
+                            "Sequence".to_string(),
+                        )),
+                    ]),
+                    ..Default::default()
+                },
+                ..settings::LinterSettings::for_rules([
+                    Rule::MissingRequiredImport,
+                    Rule::UnnecessaryFutureImport,
+                    Rule::DeprecatedImport,
+                ])
+            },
+        );
+        assert_diagnostics!(diagnostics, @r"
+        <filename>:1:1: I002 [*] Missing required import: `from __future__ import generator_stop`
+        ℹ Safe fix
+          1 |+from __future__ import generator_stop
+        1 2 | 1
+
+        <filename>:1:1: I002 [*] Missing required import: `from collections import Sequence`
+        ℹ Safe fix
+          1 |+from collections import Sequence
+        1 2 | 1
+        ");
+    }
 }
diff --git a/crates/ruff_linter/src/test.rs b/crates/ruff_linter/src/test.rs
index cf63762b8a..02eca4ca05 100644
--- a/crates/ruff_linter/src/test.rs
+++ b/crates/ruff_linter/src/test.rs
@@ -380,7 +380,7 @@ macro_rules! assert_diagnostics {
     }};
     ($value:expr, @$snapshot:literal) => {{
         insta::with_settings!({ omit_expression => true }, {
-            insta::assert_snapshot!($crate::test::print_messages(&$value), $snapshot);
+            insta::assert_snapshot!($crate::test::print_messages(&$value), @$snapshot);
         });
     }};
     ($name:expr, $value:expr) => {{
```

</details>

I guess nobody had ever tried to use the inline snapshot version of `assert_diagnostics!` because the `@` wasn't being passed along.

This test fails on `main` with the expected failure to converge, but with your fixes it yields the expected snapshot. We fix the I002 issues and don't raise UP010 or UP035. Nice work.

---

_Review requested from @ntBre by @danparizher on 2025-07-30 15:34_

---

_Renamed from "Prevent infinite fix loop between I002 and UP010 when required import is also unnecessary" to "[`pyupgrade`] Prevent infinite loop with I002 (`UP010`, `UP035`)" by @ntBre on 2025-07-30 20:26_

---

_Renamed from "[`pyupgrade`] Prevent infinite loop with I002 (`UP010`, `UP035`)" to "[`pyupgrade`] Prevent infinite loop with `I002` (`UP010`, `UP035`)" by @ntBre on 2025-07-30 20:27_

---

_@ntBre reviewed on 2025-07-30 21:02_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/deprecated_import.rs`:723 on 2025-07-30 21:02_

I'm not totally sure, but this feels like it could be overly restrictive. This should be equivalent to:

```rust
    if import_from_stmt.names.iter().any(|alias| {
        is_import_required_by_isort(checker, StmtRef::ImportFrom(import_from_stmt), alias)
    }) {
        return;
    }
```

and I'm wondering if `all` might be the better condition than `any`. In terms of a test case, I tried this:

```rust
    #[test]
    fn i002_conflict() {
        let diagnostics = test_snippet(
            "from pipes import quote, Template",
            &settings::LinterSettings {
                isort: isort::settings::Settings {
                    required_imports: BTreeSet::from_iter([
                        // https://github.com/astral-sh/ruff/issues/18729
                        NameImport::ImportFrom(MemberNameImport::member(
                            "__future__".to_string(),
                            "generator_stop".to_string(),
                        )),
                        // https://github.com/astral-sh/ruff/issues/16802
                        NameImport::ImportFrom(MemberNameImport::member(
                            "collections".to_string(),
                            "Sequence".to_string(),
                        )),
                        // Only bail out if _all_ the names in UP035 are required. `pipes.Template`
                        // isn't flagged by UP035, so requiring it shouldn't prevent `pipes.quote`
                        // from getting a diagnostic.
                        NameImport::ImportFrom(MemberNameImport::member(
                            "pipes".to_string(),
                            "Template".to_string(),
                        )),
                    ]),
                    ..Default::default()
                },
                ..settings::LinterSettings::for_rules([
                    Rule::MissingRequiredImport,
                    Rule::UnnecessaryFutureImport,
                    Rule::DeprecatedImport,
                ])
            },
        );
        assert_diagnostics!(diagnostics, @r"
        <filename>:1:1: UP035 [*] Import from `shlex` instead: `quote`
          |
        1 | from pipes import quote, Template
          | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ UP035
          |
          = help: Import from `shlex`

        ℹ Safe fix
        1   |-from pipes import quote, Template
          1 |+from pipes import Template
          2 |+from shlex import quote

        <filename>:1:1: I002 [*] Missing required import: `from __future__ import generator_stop`
        ℹ Safe fix
          1 |+from __future__ import generator_stop
        1 2 | from pipes import quote, Template

        <filename>:1:1: I002 [*] Missing required import: `from collections import Sequence`
        ℹ Safe fix
          1 |+from collections import Sequence
        1 2 | from pipes import quote, Template
        ");
    }
```

after switching to `all`. I think it makes sense to flag `quote` even if `Template` is required. What do  you think?

I think that's consistent with the `continue` instead of `return` in UP010.

---

_@ntBre reviewed on 2025-07-30 21:04_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/deprecated_import.rs`:723 on 2025-07-30 21:04_

Actually, this may just not be the right place to inject the check. Somewhere in here we should actually inspect the `alias` being replaced. That might be the more natural place to check `is_import_required_by_isort`.

---

_@danparizher reviewed on 2025-07-31 00:01_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/pyupgrade/rules/deprecated_import.rs`:723 on 2025-07-31 00:01_

I think the `continue` approach makes the most sense. I envision collecting skipped aliases and filtering them out during processing. Feel free to iterate on the commit how you see fit. 🙂

---

_Comment by @ntBre on 2025-07-31 18:43_

I pushed a few changes:
- Stored the required imports on `ImportReplacer` instead of a `HashSet` so we can avoid allocating that up front
- Pushed to `unmatched_names` instead of continuing in `partition_imports` (this was causing the `from pipes import Template` import to be dropped when removing the `quote` import)
- Combined `i002_conflict` and `up035_partial_required_import`, that's what I had in mind initially. I don't think we need both tests separately
- Tried to simplify `is_import_required_by_isort` a bit

I'm not sure if the simplification actually paid off. It seems nice not to allocate strings for the `NameImport`s, but now we have to call `qualified_name` repeatedly in the `any` check. Your version wasn't showing any performance regression, so if this regresses performance at all, I'll just put it back.

---

_Merged by @ntBre on 2025-07-31 19:17_

---

_Closed by @ntBre on 2025-07-31 19:17_

---

_Branch deleted on 2025-07-31 20:44_

---
