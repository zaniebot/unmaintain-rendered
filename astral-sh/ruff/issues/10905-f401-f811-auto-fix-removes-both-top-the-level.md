```yaml
number: 10905
title: F401 + F811 - auto-fix removes both top the level import and any inside-function import
type: issue
state: closed
author: Adamasterr
labels:
  - bug
  - fixes
assignees: []
created_at: 2024-04-12T12:27:16Z
updated_at: 2024-04-27T15:44:54Z
url: https://github.com/astral-sh/ruff/issues/10905
synced_at: 2026-01-10T11:09:53Z
```

# F401 + F811 - auto-fix removes both top the level import and any inside-function import

---

_Issue opened by @Adamasterr on 2024-04-12 12:27_

Hello!

Here's a minimal example, applying auto-fix to the following:

```py
import os  # F401


def function():
    import os  # F811

    print(os.name)
```

Results in the following:

```py
def function():

    print(os.name)  # F821, here, "os" is not defined
```

This makes for a fix that breaks the code.


<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Label `bug` added by @MichaReiser on 2024-04-12 12:32_

---

_Label `fixes` added by @MichaReiser on 2024-04-12 12:32_

---

_Comment by @charliermarsh on 2024-04-12 14:05_

Thanks, that makes sense.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-12 14:32_

---

_Comment by @AlexWaygood on 2024-04-14 11:04_

This patch fixes things, in that the two fixes don't end up creating invalid code at the end of the day when they're combined:

```diff
--- a/crates/ruff_linter/src/checkers/ast/analyze/deferred_scopes.rs
+++ b/crates/ruff_linter/src/checkers/ast/analyze/deferred_scopes.rs
@@ -1,4 +1,4 @@
-use ruff_diagnostics::{Diagnostic, Fix};
+use ruff_diagnostics::{Diagnostic, Edit, Fix};
 use ruff_python_semantic::analyze::visibility;
 use ruff_python_semantic::{Binding, BindingKind, Imported, ResolvedReference, ScopeKind};
 use ruff_text_size::Ranged;
@@ -294,9 +294,33 @@ pub(crate) fn deferred_scopes(checker: &mut Checker) {
                                             checker.stylist(),
                                             checker.indexer(),
                                         )?;
-                                        Ok(Fix::safe_edit(edit).isolate(Checker::isolation(
-                                            checker.semantic().parent_statement_id(source),
-                                        )))
+                                        // We also add a no-op edit to force conflicts with the fix for `unused-imports`.
+                                        // Consider:
+                                        // ```python
+                                        // import sys
+                                        //
+                                        // def foo():
+                                        //     import sys
+                                        //     return sys.version_info
+                                        // ```
+                                        //
+                                        // Assume you omit this no-op edit. If you run Ruff with `unused-imports` and
+                                        // `redefined-while-unused` over this snippet, it will generate two fixes:
+                                        // (1) remove the unused `sys` import; and
+                                        // (2) remove the "redundant redefinition" of `sys`, under the assumption that
+                                        //     `sys` is already imported and available.
+                                        //
+                                        // By adding this no-op edit, we force the `unused-imports` fix to conflict
+                                        // with the `redefined-while-unused` fix, and thus will avoid applying both
+                                        // fixes in the same pass.
+                                        let noop_edit = Edit::range_replacement(
+                                            checker.locator().slice(shadowed).to_string(),
+                                            shadowed.range(),
+                                        );
+                                        Ok(Fix::safe_edits(edit, std::iter::once(noop_edit))
+                                            .isolate(Checker::isolation(
+                                                checker.semantic().parent_statement_id(source),
+                                            )))
                                     });
```

(Logic inspired by https://github.com/astral-sh/ruff/blob/812b0976a9e04c6bc5256911419daf4f82de404b/crates/ruff_linter/src/importer/mod.rs#L277-L296)

It's still not ideal, though, as it ends up applying this fix:

```diff
--- foo.py
+++ foo.py
@@ -1,4 +1,3 @@
-import os  # F401
 
 
 def function():
```

when ideally it would remove the local import rather than the top-level import

---

_Comment by @charliermarsh on 2024-04-14 11:53_

Maybe we should change F811 to only remove the "shadowed" import if they're in the same scope? I need to look back at why we changed that in the first place.

---

_Closed by @charliermarsh on 2024-04-27 15:44_

---
