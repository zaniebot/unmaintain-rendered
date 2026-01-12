```yaml
number: 16012
title: "Pass `Checker` by immutable reference to lint rules"
type: pull_request
state: merged
author: dylwil3
labels:
  - internal
assignees: []
merged: true
base: main
head: checker-mut
created_at: 2025-02-07T05:43:56Z
updated_at: 2025-02-07T15:05:50Z
url: https://github.com/astral-sh/ruff/pull/16012
synced_at: 2026-01-12T15:55:53Z
```

# Pass `Checker` by immutable reference to lint rules

---

_@dylwil3_

This very large PR changes the field `.diagnostics` in the `Checker` from a `Vec<Diagnostic>` to a `RefCell<Vec<Diagnostic>>`, adds methods to push new diagnostics to this cell, and then removes unnecessary mutability throughout all of our lint rule implementations.

Consequently, the compiler may now enforce what was, till now, the _convention_ that the only changes to the `Checker` that can happen during a lint are the addition of diagnostics[^1].

The PR is best reviewed commit-by-commit. I have tried to keep the large commits limited to "bulk actions that you can easily see are performing the same find/replace on a large number of files", and separate anything ad-hoc or with larger diffs. Please let me know if there's anything else I can do to make this easier to review!

Many thanks to [`ast-grep`](https://github.com/ast-grep/ast-grep), [`helix`](https://github.com/helix-editor/helix), and good ol' fashioned`git` magic, without which this PR would have taken the rest of my natural life.

[^1]: And randomly also the seen variables violating `flake8-bugbear`?

---

_Label `internal` added by @dylwil3 on 2025-02-07 05:43_

---

_Review requested from @AlexWaygood by @dylwil3 on 2025-02-07 05:43_

---

_Comment by @github-actions[bot] on 2025-02-07 05:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/c5f4a7f302f3ef763ab3deacd01cb0314789d7c7/superset/views/utils.py#L1'>superset/views/utils.py:1:1:</a> CPY001 Missing copyright notice at top of file
- <a href='https://github.com/apache/superset/blob/c5f4a7f302f3ef763ab3deacd01cb0314789d7c7/superset/views/utils.py#L1'>superset/views/utils.py:1:1:</a> CPY001 Missing copyright notice at top of file
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| CPY001 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:494 on 2025-02-07 07:31_

We may want to rename this method to `report_type_diagnostic` and also change its signature to take a `&self` (unless you've already done so in a higher up commit)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:2583 on 2025-02-07 07:32_

Nit: We could add a `report_diagnostic_mut` method to discourage direct `self.diagnostics` accessors (I'm afraid from people copying this code over using `self.report_diagnostic`)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/ambiguous_unicode_character.rs`:201 on 2025-02-07 07:33_

This will be slightly less performant because we now allocate a temporary `Vec` vs just writing to the `checker.diagnostics`. I think my preference here is to just pass `&Checker` (and remove the `checker.settings` argument.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:16 on 2025-02-07 07:36_

Nice!

---

_@MichaReiser approved on 2025-02-07 07:37_

Thank you. This is great, and thanks for taking the time to split the refactor into individual commits. It made reviewing this PR easy, even though it touches on so many files because it allowed me to skip over the "mundane" commits that touch most files. 

I only have a few more minor comments. Would you happen to know what's going on with the ecosystem check? I'm surprised why it adds and removes a diagnostic. But maybe that's because we now return the diagnostics in a different order.



---

_@dylwil3 reviewed on 2025-02-07 08:03_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/rules/ambiguous_unicode_character.rs`:201 on 2025-02-07 08:03_

I tried to say this in the (extended) commit message but I think I didn't communicate it well:

The helper `ambiguous_unicode_character` is used in both AST rules and a Tokens rule. The tokens rules predate the checker so I'd have to inline the logic, or make two helper functions, or do some other change, which is why I resisted.

But if you think the performance gain is worth splitting up the tokens vs. AST logic I can still do that!

---

_@MichaReiser reviewed on 2025-02-07 08:05_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/ambiguous_unicode_character.rs`:201 on 2025-02-07 08:05_

I must admit, I didn't read the commit messages in detail. Shame on me! 

I think it's fine then. We could do something similar to Red Knot where we have a trimmed down struct for any context that can emit diagnostics but it feels overkill here.

---

_@dylwil3 reviewed on 2025-02-07 08:08_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/checkers/ast/mod.rs`:2583 on 2025-02-07 08:08_

Should we just make `diagnostics` (and `flake8_bugbear_seen`) private? I just checked and the code compiles if I delete `pub(crate)`.

---

_@MichaReiser reviewed on 2025-02-07 08:10_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:2583 on 2025-02-07 08:10_

Oh yes, please!

---

_Comment by @dylwil3 on 2025-02-07 08:22_

Thanks so much for reviewing this Micha! 

I'm gonna look into the weird ecosystem thing in the morning... er... later _this_ morning. Wow I should go to sleep...

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/mod.rs`:270 on 2025-02-07 11:37_

I believe this could just be:

```suggestion
            diagnostics: RefCell::default(),
            flake8_bugbear_seen: RefCell::default(),
```

But happy to stick with what you currently have as well!

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:209 on 2025-02-07 11:46_

I think the only reason we previously collected the diagnostics into an intermediate `Vec` in this rule before adding them to `checker.diagnostics` was to avoid some annoying borrow-checker issues. With this PR those borrow-checker issues are gone, so we can just do this:

```diff
--- a/crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs
+++ b/crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs
@@ -163,7 +163,6 @@ pub(crate) fn fastapi_unused_path_parameter(
     }
 
     // Check if any of the path parameters are not in the function signature.
-    let mut diagnostics = vec![];
     for (path_param, range) in path_params {
         // Ignore invalid identifiers (e.g., `user-id`, as opposed to `user_id`)
         if !is_identifier(path_param) {
@@ -203,10 +202,8 @@ pub(crate) fn fastapi_unused_path_parameter(
                 checker.locator().contents(),
             )));
         }
-        diagnostics.push(diagnostic);
+        checker.report_diagnostic(diagnostic);
     }
-
-    checker.report_diagnostics(diagnostics);
 }
 
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_unused_arguments/rules/unused_arguments.rs`:408 on 2025-02-07 12:56_

I think you could avoid the intermediate allocation here by passing `Checker` around everywhere (and make a few other simplifications while you're about it). This could be a followup, though

<details>

```diff
diff --git a/crates/ruff_linter/src/rules/flake8_unused_arguments/rules/unused_arguments.rs b/crates/ruff_linter/src/rules/flake8_unused_arguments/rules/unused_arguments.rs
index cda67c264..8af376968 100644
--- a/crates/ruff_linter/src/rules/flake8_unused_arguments/rules/unused_arguments.rs
+++ b/crates/ruff_linter/src/rules/flake8_unused_arguments/rules/unused_arguments.rs
@@ -1,4 +1,3 @@
-use regex::Regex;
 use ruff_python_ast as ast;
 use ruff_python_ast::{Parameter, Parameters, Stmt, StmtExpr, StmtFunctionDef, StmtRaise};
 
@@ -246,15 +245,11 @@ impl Argumentable {
 }
 
 /// Check a plain function for unused arguments.
-fn function(
-    argumentable: Argumentable,
-    parameters: &Parameters,
-    scope: &Scope,
-    semantic: &SemanticModel,
-    dummy_variable_rgx: &Regex,
-    ignore_variadic_names: bool,
-    diagnostics: &mut Vec<Diagnostic>,
-) {
+fn function(argumentable: Argumentable, parameters: &Parameters, scope: &Scope, checker: &Checker) {
+    let ignore_variadic_names = checker
+        .settings
+        .flake8_unused_arguments
+        .ignore_variadic_names;
     let args = parameters
         .iter_non_variadic_params()
         .map(|parameter_with_default| &parameter_with_default.parameter)
@@ -272,26 +267,15 @@ fn function(
                 .into_iter()
                 .skip(usize::from(ignore_variadic_names)),
         );
-    call(
-        argumentable,
-        args,
-        scope,
-        semantic,
-        dummy_variable_rgx,
-        diagnostics,
-    );
+    call(argumentable, args, scope, checker);
 }
 
 /// Check a method for unused arguments.
-fn method(
-    argumentable: Argumentable,
-    parameters: &Parameters,
-    scope: &Scope,
-    semantic: &SemanticModel,
-    dummy_variable_rgx: &Regex,
-    ignore_variadic_names: bool,
-    diagnostics: &mut Vec<Diagnostic>,
-) {
+fn method(argumentable: Argumentable, parameters: &Parameters, scope: &Scope, checker: &Checker) {
+    let ignore_variadic_names = checker
+        .settings
+        .flake8_unused_arguments
+        .ignore_variadic_names;
     let args = parameters
         .iter_non_variadic_params()
         .skip(1)
@@ -310,25 +294,18 @@ fn method(
                 .into_iter()
                 .skip(usize::from(ignore_variadic_names)),
         );
-    call(
-        argumentable,
-        args,
-        scope,
-        semantic,
-        dummy_variable_rgx,
-        diagnostics,
-    );
+    call(argumentable, args, scope, checker);
 }
 
 fn call<'a>(
     argumentable: Argumentable,
     parameters: impl Iterator<Item = &'a Parameter>,
     scope: &Scope,
-    semantic: &SemanticModel,
-    dummy_variable_rgx: &Regex,
-    diagnostics: &mut Vec<Diagnostic>,
+    checker: &Checker,
 ) {
-    diagnostics.extend(parameters.filter_map(|arg| {
+    let semantic = checker.semantic();
+    let dummy_variable_rgx = &checker.settings.dummy_variable_rgx;
+    checker.report_diagnostics(parameters.filter_map(|arg| {
         let binding = scope
             .get(arg.name())
             .map(|binding_id| semantic.binding(binding_id))?;
@@ -405,7 +382,6 @@ pub(crate) fn is_not_implemented_stub_with_variable(
 
 /// ARG001, ARG002, ARG003, ARG004, ARG005
 pub(crate) fn unused_arguments(checker: &Checker, scope: &Scope) {
-    let mut diagnostics = Vec::new();
     if scope.uses_locals() {
         return;
     }
@@ -437,18 +413,7 @@ pub(crate) fn unused_arguments(checker: &Checker, scope: &Scope) {
                         && !is_not_implemented_stub_with_variable(function_def, checker.semantic())
                         && !visibility::is_overload(decorator_list, checker.semantic())
                     {
-                        function(
-                            Argumentable::Function,
-                            parameters,
-                            scope,
-                            checker.semantic(),
-                            &checker.settings.dummy_variable_rgx,
-                            checker
-                                .settings
-                                .flake8_unused_arguments
-                                .ignore_variadic_names,
-                            &mut diagnostics,
-                        );
+                        function(Argumentable::Function, parameters, scope, checker);
                     }
                 }
                 function_type::FunctionType::Method => {
@@ -463,18 +428,7 @@ pub(crate) fn unused_arguments(checker: &Checker, scope: &Scope) {
                         && !visibility::is_override(decorator_list, checker.semantic())
                         && !visibility::is_overload(decorator_list, checker.semantic())
                     {
-                        method(
-                            Argumentable::Method,
-                            parameters,
-                            scope,
-                            checker.semantic(),
-                            &checker.settings.dummy_variable_rgx,
-                            checker
-                                .settings
-                                .flake8_unused_arguments
-                                .ignore_variadic_names,
-                            &mut diagnostics,
-                        );
+                        method(Argumentable::Method, parameters, scope, checker);
                     }
                 }
                 function_type::FunctionType::ClassMethod => {
@@ -489,18 +443,7 @@ pub(crate) fn unused_arguments(checker: &Checker, scope: &Scope) {
                         && !visibility::is_override(decorator_list, checker.semantic())
                         && !visibility::is_overload(decorator_list, checker.semantic())
                     {
-                        method(
-                            Argumentable::ClassMethod,
-                            parameters,
-                            scope,
-                            checker.semantic(),
-                            &checker.settings.dummy_variable_rgx,
-                            checker
-                                .settings
-                                .flake8_unused_arguments
-                                .ignore_variadic_names,
-                            &mut diagnostics,
-                        );
+                        method(Argumentable::ClassMethod, parameters, scope, checker);
                     }
                 }
                 function_type::FunctionType::StaticMethod => {
@@ -515,18 +458,7 @@ pub(crate) fn unused_arguments(checker: &Checker, scope: &Scope) {
                         && !visibility::is_override(decorator_list, checker.semantic())
                         && !visibility::is_overload(decorator_list, checker.semantic())
                     {
-                        function(
-                            Argumentable::StaticMethod,
-                            parameters,
-                            scope,
-                            checker.semantic(),
-                            &checker.settings.dummy_variable_rgx,
-                            checker
-                                .settings
-                                .flake8_unused_arguments
-                                .ignore_variadic_names,
-                            &mut diagnostics,
-                        );
+                        function(Argumentable::StaticMethod, parameters, scope, checker);
                     }
                 }
             }
@@ -534,22 +466,10 @@ pub(crate) fn unused_arguments(checker: &Checker, scope: &Scope) {
         ScopeKind::Lambda(ast::ExprLambda { parameters, .. }) => {
             if let Some(parameters) = parameters {
                 if checker.enabled(Argumentable::Lambda.rule_code()) {
-                    function(
-                        Argumentable::Lambda,
-                        parameters,
-                        scope,
-                        checker.semantic(),
-                        &checker.settings.dummy_variable_rgx,
-                        checker
-                            .settings
-                            .flake8_unused_arguments
-                            .ignore_variadic_names,
-                        &mut diagnostics,
-                    );
+                    function(Argumentable::Lambda, parameters, scope, checker);
                 }
             }
         }
         _ => panic!("Expected ScopeKind::Function | ScopeKind::Lambda"),
     }
-    checker.report_diagnostics(diagnostics);
 }
```

</details>

---

_@AlexWaygood approved on 2025-02-07 13:14_

This is fantastic. We should have done this a long time ago -- great work!!

---

_@dylwil3 reviewed on 2025-02-07 14:33_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_unused_arguments/rules/unused_arguments.rs`:408 on 2025-02-07 14:33_

you make it so easy when I can just copy/paste your patch, thank you!

---

_Merged by @dylwil3 on 2025-02-07 15:05_

---

_Closed by @dylwil3 on 2025-02-07 15:05_

---
