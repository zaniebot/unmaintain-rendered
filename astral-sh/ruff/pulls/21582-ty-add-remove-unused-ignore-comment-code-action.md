```yaml
number: 21582
title: "[ty] Add 'remove unused ignore comment' code action"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/add-ignore-action
created_at: 2025-11-22T21:29:52Z
updated_at: 2025-11-25T13:08:24Z
url: https://github.com/astral-sh/ruff/pull/21582
synced_at: 2026-01-12T15:57:28Z
```

# [ty] Add 'remove unused ignore comment' code action

---

_@MichaReiser_

## Summary

This PR adds a code action to remove unused ignore comments.

This PR also includes some infrastructure boilerplate to set up code actions in the editor:

* Extend `snapshot-diagnostics` to render fixes
* Render fixes when using `--output-format=full`
* Hook up edits and the code action request in the LSP
* Add the `Unnecessary` tag to `unused-ignore-comment` diagnostics
* Group multiple unused codes into a single diagnostic

The same fix can be used on the CLI once we add `ty fix` 

Note: `unused-ignore-comment` is currently disabled by default.

https://github.com/user-attachments/assets/f9e21087-3513-4156-85d7-a90b1a7a3489


 

---

_Label `server` added by @MichaReiser on 2025-11-22 21:29_

---

_Label `ty` added by @MichaReiser on 2025-11-22 21:29_

---

_Renamed from "[ty] Add 'remove unused ignore comment' code action"" to "[ty] Add 'remove unused ignore comment' code action" by @MichaReiser on 2025-11-22 21:29_

---

_Comment by @astral-sh-bot[bot] on 2025-11-22 21:44_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-22 21:59_


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

_Comment by @astral-sh-bot[bot] on 2025-11-23 10:12_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
- pytest_robotframework/_internal/pytest/plugin.py:127:99: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'unresolved-attribute'
+ pytest_robotframework/_internal/pytest/plugin.py:127:99: warning[unused-ignore-comment] Unused `ty: ignore` directive
- pytest_robotframework/_internal/pytest/plugin.py:197:93: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'unresolved-attribute'
+ pytest_robotframework/_internal/pytest/plugin.py:197:93: warning[unused-ignore-comment] Unused `ty: ignore` directive
- pytest_robotframework/_internal/pytest/plugin.py:235:89: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'non-subscriptable'
+ pytest_robotframework/_internal/pytest/plugin.py:235:89: warning[unused-ignore-comment] Unused `ty: ignore` directive
- pytest_robotframework/_internal/robot/library.py:86:99: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'possibly-missing-attribute'
+ pytest_robotframework/_internal/robot/library.py:86:99: warning[unused-ignore-comment] Unused `ty: ignore` directive
- pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:455:40: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'missing-typed-dict-key'
- pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:455:63: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'invalid-argument-type'
+ pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:455:28: warning[unused-ignore-comment] Unused `ty: ignore` directive
- pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:472:40: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'missing-typed-dict-key'
- pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:472:63: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'invalid-argument-type'
+ pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:472:28: warning[unused-ignore-comment] Unused `ty: ignore` directive
- tests/conftest.py:82:77: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'subclass-of-final-class'
+ tests/conftest.py:82:77: warning[unused-ignore-comment] Unused `ty: ignore` directive
- Found 169 diagnostics
+ Found 167 diagnostics

typeshed-stats (https://github.com/AlexWaygood/typeshed-stats)
- tests/test__cli.py:541:38: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'invalid-assignment'
+ tests/test__cli.py:541:38: warning[unused-ignore-comment] Unused `ty: ignore` directive

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 45 diagnostics
+ Found 44 diagnostics

streamlit (https://github.com/streamlit/streamlit)
- lib/streamlit/config.py:2635:37: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'invalid-return-type'
+ lib/streamlit/config.py:2635:37: warning[unused-ignore-comment] Unused `ty: ignore` directive
- lib/streamlit/connections/sql_connection.py:221:37: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'redundant-cast'
+ lib/streamlit/connections/sql_connection.py:221:37: warning[unused-ignore-comment] Unused `ty: ignore` directive
- lib/streamlit/dataframe_util.py:701:41: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'redundant-cast'
+ lib/streamlit/dataframe_util.py:701:41: warning[unused-ignore-comment] Unused `ty: ignore` directive
- lib/streamlit/dataframe_util.py:730:41: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'redundant-cast'
+ lib/streamlit/dataframe_util.py:730:41: warning[unused-ignore-comment] Unused `ty: ignore` directive
- lib/streamlit/elements/lib/built_in_chart_utils.py:637:22: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'invalid-return-type'
+ lib/streamlit/elements/lib/built_in_chart_utils.py:637:22: warning[unused-ignore-comment] Unused `ty: ignore` directive
- lib/streamlit/elements/vega_charts.py:382:33: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'unresolved-attribute'
+ lib/streamlit/elements/vega_charts.py:382:33: warning[unused-ignore-comment] Unused `ty: ignore` directive
- lib/streamlit/elements/vega_charts.py:416:30: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'invalid-context-manager'
+ lib/streamlit/elements/vega_charts.py:416:30: warning[unused-ignore-comment] Unused `ty: ignore` directive
- lib/streamlit/elements/vega_charts.py:418:37: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'invalid-context-manager'
+ lib/streamlit/elements/vega_charts.py:418:37: warning[unused-ignore-comment] Unused `ty: ignore` directive
- lib/streamlit/elements/widgets/data_editor.py:171:67: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'possibly-missing-attribute'
+ lib/streamlit/elements/widgets/data_editor.py:171:67: warning[unused-ignore-comment] Unused `ty: ignore` directive
- lib/streamlit/elements/widgets/slider.py:801:63: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'unsupported-operator'
+ lib/streamlit/elements/widgets/slider.py:801:63: warning[unused-ignore-comment] Unused `ty: ignore` directive
- lib/streamlit/elements/widgets/time_widgets.py:167:73: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'invalid-argument-type'
+ lib/streamlit/elements/widgets/time_widgets.py:167:73: warning[unused-ignore-comment] Unused `ty: ignore` directive
- lib/streamlit/file_util.py:247:51: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'no-matching-overload'
+ lib/streamlit/file_util.py:247:51: warning[unused-ignore-comment] Unused `ty: ignore` directive
- lib/streamlit/runtime/caching/cache_utils.py:128:39: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'invalid-argument-type'
+ lib/streamlit/runtime/caching/cache_utils.py:128:39: warning[unused-ignore-comment] Unused `ty: ignore` directive
- lib/streamlit/runtime/caching/cache_utils.py:174:40: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'invalid-argument-type'
+ lib/streamlit/runtime/caching/cache_utils.py:174:40: warning[unused-ignore-comment] Unused `ty: ignore` directive
- lib/streamlit/runtime/caching/cache_utils.py:200:35: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'invalid-argument-type'
+ lib/streamlit/runtime/caching/cache_utils.py:200:35: warning[unused-ignore-comment] Unused `ty: ignore` directive

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/core/groupby/generic.pyi:215:34: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'invalid-argument-type'
+ pandas-stubs/core/groupby/generic.pyi:215:34: warning[unused-ignore-comment] Unused `ty: ignore` directive
- pandas-stubs/core/groupby/generic.pyi:220:34: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'invalid-argument-type'
+ pandas-stubs/core/groupby/generic.pyi:220:34: warning[unused-ignore-comment] Unused `ty: ignore` directive
- pandas-stubs/core/groupby/generic.pyi:225:34: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'invalid-argument-type'
+ pandas-stubs/core/groupby/generic.pyi:225:34: warning[unused-ignore-comment] Unused `ty: ignore` directive


```

</details>


No memory usage changes detected ✅



---

_@MichaReiser reviewed on 2025-11-23 11:33_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/Cargo.toml`:16 on 2025-11-23 11:33_

We should probably fold `ruff_diagnostics` into `ruff_db`. There's really not much in there but this seemed unrelated to this PR

---

_@MichaReiser reviewed on 2025-11-23 11:40_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/suppression.rs`:642 on 2025-11-23 11:40_

This is now handled by the unused suppression lint itself

---

_Marked ready for review by @MichaReiser on 2025-11-23 11:47_

---

_Review requested from @carljm by @MichaReiser on 2025-11-23 11:47_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-11-23 11:47_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-23 11:47_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-23 11:47_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/suppressions/ty_ignore.md`:134 on 2025-11-23 16:17_

the comment here suggests to me that the trailing whitespace might have been intentional :P

---

_@AlexWaygood reviewed on 2025-11-23 16:17_

---

_@MichaReiser reviewed on 2025-11-23 16:20_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/suppressions/ty_ignore.md`:134 on 2025-11-23 16:20_

Thanks, yes they are (which is why I added the comment). It's so annoying that I can't tell my editors: Don't trim trailing whitespaces here

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/suppression.rs`:189 on 2025-11-23 16:21_

nit: is there a reason to import this locally rather than globally?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/suppression.rs`:339 on 2025-11-23 16:29_

you can simplify the code by using the `DiagnosticGuard::help()` method:

```diff
diff --git a/crates/ty_python_semantic/src/suppression.rs b/crates/ty_python_semantic/src/suppression.rs
index caa9796295..16db479d13 100644
--- a/crates/ty_python_semantic/src/suppression.rs
+++ b/crates/ty_python_semantic/src/suppression.rs
@@ -3,8 +3,7 @@ use crate::lint::{GetLintError, Level, LintMetadata, LintRegistry, LintStatus};
 use crate::types::TypeCheckDiagnostics;
 use crate::{Db, declare_lint, lint::LintId};
 use ruff_db::diagnostic::{
-    Annotation, Diagnostic, DiagnosticId, IntoDiagnosticMessage, Severity, Span, SubDiagnostic,
-    SubDiagnosticSeverity,
+    Annotation, Diagnostic, DiagnosticId, IntoDiagnosticMessage, Severity, Span,
 };
 use ruff_db::{files::File, parsed::parsed_module, source::source_text};
 use ruff_diagnostics::{Edit, Fix};
@@ -332,17 +331,11 @@ fn check_unused_suppressions(context: &mut CheckSuppressionsContext) {
                         );
                         diag.set_fix(Fix::safe_edit(Edit::range_deletion(fix_range)));
 
-                        if unused_codes.is_empty() {
-                            diag.sub(SubDiagnostic::new(
-                                SubDiagnosticSeverity::Help,
-                                "Remove the unused suppression code",
-                            ));
+                        diag.help(if unused_codes.is_empty() {
+                            "Remove the unused suppression code"
                         } else {
-                            diag.sub(SubDiagnostic::new(
-                                SubDiagnosticSeverity::Help,
-                                "Remove the unused suppression codes",
-                            ));
-                        }
+                            "Remove the unused suppression codes"
+                        });
                     }
 
                     continue;
@@ -377,10 +370,7 @@ fn check_unused_suppressions(context: &mut CheckSuppressionsContext) {
             .unwrap()
             .push_tag(ruff_db::diagnostic::DiagnosticTag::Unnecessary);
         diag.set_fix(remove_comment_fix(suppression, &source));
-        diag.sub(SubDiagnostic::new(
-            SubDiagnosticSeverity::Help,
-            "Remove the unused suppression comment",
-        ));
+        diag.help("Remove the unused suppression comment");
     }
 }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/suppression.rs`:1092 on 2025-11-23 16:30_

```suggestion
            comment_end,
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/suppression.rs`:1112 on 2025-11-23 16:31_

```suggestion
        comment_end,
```

---

_@AlexWaygood reviewed on 2025-11-23 16:33_

Very excited to see this happen! A few nits from skimming

---

_Review request for @carljm removed by @carljm on 2025-11-24 22:30_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-11-25 08:10_

---

_Review request for @dcreager removed by @MichaReiser on 2025-11-25 08:10_

---

_Review requested from @Gankra by @MichaReiser on 2025-11-25 08:10_

---

_@MichaReiser reviewed on 2025-11-25 08:17_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/suppression.rs`:189 on 2025-11-25 08:17_

Mainly laziness. RustRover can't auto import `Write` and having to scroll to the top and down again breaks my entire flow.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/suppression.rs`:300 on 2025-11-25 11:06_

you could use early-return style here to reduce the indentation:

```diff
diff --git a/crates/ty_python_semantic/src/suppression.rs b/crates/ty_python_semantic/src/suppression.rs
index ab246c1b0a..852fa5e1bd 100644
--- a/crates/ty_python_semantic/src/suppression.rs
+++ b/crates/ty_python_semantic/src/suppression.rs
@@ -294,49 +294,51 @@ fn check_unused_suppressions(context: &mut CheckSuppressionsContext) {
                         let _ = write!(&mut codes, ", '{code}'", code = code.name());
                     }
 
-                    if let Some(diag) = context.report_unchecked(
+                    let Some(diag) = context.report_unchecked(
                         &UNUSED_IGNORE_COMMENT,
                         TextRange::new(suppression.range.start(), current.range.end()),
-                    ) {
-                        let mut diag = diag.into_diagnostic(format_args!(
-                            "Unused `{kind}` directive: {codes}",
-                            kind = suppression.kind
-                        ));
-
-                        diag.primary_annotation_mut()
-                            .unwrap()
-                            .push_tag(ruff_db::diagnostic::DiagnosticTag::Unnecessary);
-
-                        // Delete everything up to the start of the next code.
-                        let trailing_len: TextSize = source[current.range.end().to_usize()..]
+                    ) else {
+                        continue;
+                    };
+
+                    let mut diag = diag.into_diagnostic(format_args!(
+                        "Unused `{kind}` directive: {codes}",
+                        kind = suppression.kind
+                    ));
+
+                    diag.primary_annotation_mut()
+                        .unwrap()
+                        .push_tag(ruff_db::diagnostic::DiagnosticTag::Unnecessary);
+
+                    // Delete everything up to the start of the next code.
+                    let trailing_len: TextSize = source[current.range.end().to_usize()..]
+                        .chars()
+                        .take_while(|c: &char| c.is_whitespace() || *c == ',')
+                        .map(TextLen::text_len)
+                        .sum();
+
+                    // If we delete the last codes before `]`, ensure we delete any trailing comma
+                    let leading_len: TextSize = if includes_last_code {
+                        source[..suppression.range.start().to_usize()]
                             .chars()
+                            .rev()
                             .take_while(|c: &char| c.is_whitespace() || *c == ',')
                             .map(TextLen::text_len)
-                            .sum();
-
-                        // If we delete the last codes before `]`, ensure we delete any trailing comma
-                        let leading_len: TextSize = if includes_last_code {
-                            source[..suppression.range.start().to_usize()]
-                                .chars()
-                                .rev()
-                                .take_while(|c: &char| c.is_whitespace() || *c == ',')
-                                .map(TextLen::text_len)
-                                .sum()
-                        } else {
-                            TextSize::default()
-                        };
-
-                        let fix_range = TextRange::new(
-                            suppression.range.start() - leading_len,
-                            current.range.end() + trailing_len,
-                        );
-                        diag.set_fix(Fix::safe_edit(Edit::range_deletion(fix_range)));
-
-                        if unused_codes.is_empty() {
-                            diag.help("Remove the unused suppression code");
-                        } else {
-                            diag.help("Remove the unused suppression codes");
-                        }
+                            .sum()
+                    } else {
+                        TextSize::default()
+                    };
+
+                    let fix_range = TextRange::new(
+                        suppression.range.start() - leading_len,
+                        current.range.end() + trailing_len,
+                    );
+                    diag.set_fix(Fix::safe_edit(Edit::range_deletion(fix_range)));
+
+                    if unused_codes.is_empty() {
+                        diag.help("Remove the unused suppression code");
+                    } else {
+                        diag.help("Remove the unused suppression codes");
                     }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/suppression.rs`:333 on 2025-11-25 11:09_

`type: ignore` comments, unlike `ty: ignore` comments, might have other consumers apart from ty (the user might run multiple type checkers over their codebase, especially if they're maintaining a library). So I'd lean towards marking this an unsafe fix for now.

I know we've discussed that we want better configuration options around whether ty should respect `type: ignore` comments at all (or only look at `ty: ignore` comments, and if we had those configuration options I think this could possibly be made a safe fix... but we don't have those configuration options yet ;)

---

_@AlexWaygood approved on 2025-11-25 11:10_

🚀

(Didn't review the `ty_server` code in depth)

---

_@MichaReiser reviewed on 2025-11-25 12:08_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/suppression.rs`:333 on 2025-11-25 12:08_

I'd prefer to solve this as part of adding a new setting or splitting unused ignore comment into multiple rules. 



---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/suppression.rs`:300 on 2025-11-25 12:09_

I prefer how it is now. It makes it clearer that all code paths in this branch ultimately call `continue`

---

_@MichaReiser reviewed on 2025-11-25 12:09_

---

_@AlexWaygood reviewed on 2025-11-25 12:11_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/suppression.rs`:333 on 2025-11-25 12:11_

I agree we should do that. To me, marking these as unsafe fixes in the meantime seems like the fairer thing to do for our users until we do that.

---

_@MichaReiser reviewed on 2025-11-25 12:19_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/suppression.rs`:333 on 2025-11-25 12:19_

You can't really enable `unused-ignore-comment` if you're using another type checker anyway, we don't support a `--fix` command, nor applicabilities, and triggering it from the IDE is a manual action. 


---

_Review comment by @Gankra on `crates/ty_python_semantic/Cargo.toml`:16 on 2025-11-25 13:02_

I tried this and it ran into circular reference nastiness (stuff depends on `ruff_diagnostics` that `ruff_db` in turn depends on, I guess?). I opted to just `pub use` the relevant items in `ruff_db`.

---

_@Gankra approved on 2025-11-25 13:07_

Rad, yeah this is exactly the minimal version that makes sense.

---

_Merged by @Gankra on 2025-11-25 13:08_

---

_Closed by @Gankra on 2025-11-25 13:08_

---

_Branch deleted on 2025-11-25 13:08_

---
