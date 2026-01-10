```yaml
number: 15462
title: "[`FastAPI`] Update `Annotated` fixes (`FAST002`)"
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: brent/fix-15043
created_at: 2025-01-13T22:26:17Z
updated_at: 2025-01-15T18:05:55Z
url: https://github.com/astral-sh/ruff/pull/15462
synced_at: 2026-01-10T20:34:00Z
```

# [`FastAPI`] Update `Annotated` fixes (`FAST002`)

---

_Pull request opened by @ntBre on 2025-01-13 22:26_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
The initial purpose was to fix #15043, where code like this:
```python
from fastapi import FastAPI, Query

app = FastAPI()

@app.get("/test")
def handler(echo: str = Query("")):
    return echo
```

was being fixed to the invalid code below:

```python
from typing import Annotated
from fastapi import FastAPI, Query

app = FastAPI()

@app.get("/test")
def handler(echo: Annotated[str, Query("")]): # changed
    return echo
```

As @MichaReiser pointed out, the correct fix is:

```python
from typing import Annotated
from fastapi import FastAPI, Query

app = FastAPI()

@app.get("/test")
def handler(echo: Annotated[str, Query()] = ""): # changed
    return echo 
```

After fixing the issue for `Query`, I realized that other classes like `Path`, `Body`, `Cookie`, `Header`, `File`, and `Form` also looked susceptible to this issue. The last few commits should handle these too, which I think means this will also close #12913.

I had to reorder the arguments to the `do_stuff` test case because the new fix removes some default argument values (eg for `Path`: `some_path_param: str = Path()` becomes `some_path_param: Annotated[str, Path()]`). 

There's also #14484 related to this rule. I'm happy to take a stab at that here or in a follow up PR too.

## Test Plan

<!-- How was it tested? -->
`cargo test`

I also checked the fixed output with `uv run --with fastapi FAST002_0.py`, but it required making a bunch of additional changes to the test file that I wasn't sure we wanted in this PR.

---

_Comment by @github-actions[bot] on 2025-01-13 22:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @AlexWaygood on 2025-01-14 07:49_

---

_Label `fixes` added by @AlexWaygood on 2025-01-14 07:50_

---

_Label `bug` added by @MichaReiser on 2025-01-14 07:51_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs`:205 on 2025-01-14 07:56_

What's the reason for only setting `is_default` to `true` if `is_dep` is true? What if a non `is_dep` argument has a default value? 

If it's possible to set `seen_default` to `true` whenever there's a default value, I then suggest moving `seen_default` out of this function and simplify it to always compute it on line 115 (on the call site)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs`:150 on 2025-01-14 07:57_

Nit: It isn't very obvious what the boolean return value suggests. My first assumption would be that it returns `true` if the diagnostic was created and `false` otherwise but that's not the case. We should either consider splitting the method or at least document the return value.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs`:231 on 2025-01-14 08:02_

I try to use `unwrap` if possible because it requires readers to think through why `unwrap` is correct to do so and it's also very easy to violate the correctness constraint when refactoring code. At least if possible without requiring too much restructuring.
```suggestion
            let content = match default_arg {
                Some(default_arg) if is_dep => {
                    let default_arg = default_arg.unwrap();
                    let kwarg_list = match kwarg_list {
                        Some(v) => v.join(", "),
                        None => String::new(),
                    };
                    seen_default = true;
                    format!(
                        "{}: {}[{}, {}({kwarg_list})] = {}",
                        &parameter.parameter.name.id,
                        binding,
                        checker.locator().slice(annotation.range()),
                        checker.locator().slice(map_callable(default).range()),
                        checker.locator().slice(default_arg.value().range()),
                    )
                },
                None => {
                    if !seen_default {
                        format!(
                            "{}: {}[{}, {}]",
                            parameter.parameter.name.id,
                            binding,
                            checker.locator().slice(annotation.range()),
                            checker.locator().slice(default.range())
                        )
                    } else {
                        return Ok(None);
                    }
            };
            
            let parameter_edit = Edit::range_replacement(content, parameter.range);
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs`:170 on 2025-01-14 08:05_

We'll fail to update `seen_default` if adding the import fails. That's probably fine because it's unlikely that importing the symbol will succeed for the next parameter but it might proof a footgun in the future if we happen to add other code paths that bail early.

Should we move the `parameter_edit` generation out of the `try_set_optional_fix` to ensure it always runs to completion? (unless we manage to find a way to move `seen_default` out of `create_diagnostic` which would be my preferred solution)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs`:219 on 2025-01-14 08:13_

This is sort of hard to read. It would help readability to use named arguments
```suggestion
                    "{parameter_name}: {binding}[{annotation}, {default}({kwarg_list})] = {default_value}",
                    parameter_name = &parameter.parameter.name.id,
                    annotation = checker.locator().slice(annotation.range()),
                    metadata = checker.locator().slice(map_callable(default).range()),
                    default_valuechecker.locator().slice(default_arg.value().range()),
                )
```



---

_@MichaReiser reviewed on 2025-01-14 08:14_

---

_@MichaReiser approved on 2025-01-14 08:14_

Thanks. This overall looks good. I've a few nit improvements

---

_@AlexWaygood reviewed on 2025-01-14 12:09_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs`:231 on 2025-01-14 12:09_

> I try to use `unwrap` if possible

I think you mean that you try to _avoid_ using `unwrap` if possible :-)

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-01-14 12:09_

---

_@MichaReiser reviewed on 2025-01-14 12:15_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs`:231 on 2025-01-14 12:15_

whoops :)

---

_@ntBre reviewed on 2025-01-14 13:29_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs`:205 on 2025-01-14 13:29_

Computing it at the call site is how the original code was, but I think now we need more information (both `is_dep && default_arg.is_some()`) to know if the current fix will lead to a default argument. The examples I'm trying to handle here are cases like `some_path_param: str = Path() ` becoming `some_path_param: Annotated[str, Path()]`, where the initial presence of a default argument, which is checkable at the call site, doesn't determine the final presence of a default. `is_dep` might be a bad name for this, at a minimum.

Totally non-`is_dep` arguments (failing the `is_fastapi_dependency` check) are handled in the caller at 118.

Does that make sense? I felt like I was over-complicating this yesterday, so the answer could definitely be "no," but it still looks right to me today.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs`:150 on 2025-01-14 13:31_

Would it be any better to use a `&mut bool`? That's another option I considered, which might make it slightly more clear. I can definitely document it either way, though.

---

_@ntBre reviewed on 2025-01-14 13:31_

---

_@MichaReiser reviewed on 2025-01-14 13:33_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs`:150 on 2025-01-14 13:33_

I do prefer a return value over `&mut`. Another option is to introduce a custom enum but that might over complicate things. I'm fine with just adding some documentation

---

_@ntBre reviewed on 2025-01-14 13:41_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs`:231 on 2025-01-14 13:41_

Good catch, thank you! If only we had let-chains!

---

_@ntBre reviewed on 2025-01-14 15:54_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs`:170 on 2025-01-14 15:54_

Wow good catch. Did you have something in mind here? I tried naively moving most of the `try_set_optional_fix` body out, but we need the `binding` from the import to generate `content`.

My fix now is to call the closure and check its `Result` before sending it to `try_set_optional_fix`, but that feels a bit awkward too.

---

_@MichaReiser reviewed on 2025-01-14 16:00_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs`:261 on 2025-01-14 16:00_

I don't think it's guaranteed that it's always a default when the fix failed.

After your explanation about `seen_default,` I wonder if this is even needed. What I understand is that the fix introduces a new default value and if we don't fix, then there will never be a default? (or we should just use `parameter.default.is_some()` in that case.

This also makes me wonder: What happens if we change a parameter to now have a default value where it is preceded by parameters without a default value? Can we add a test for that?

---

_@ntBre reviewed on 2025-01-14 16:25_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs`:261 on 2025-01-14 16:25_

The rule actually only triggers at all if there's a default value initially (line 112 in particular):

https://github.com/astral-sh/ruff/blob/6f7a8ad560f03638a7efe59c80a29a47da16003e/crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs#L110-L119

We just have to avoid setting `seen_default` here in case we *remove* a default. So if we tried to fix but failed, we're definitely leaving a default argument in the parameter list, if I'm understanding correctly.

I'm noticing now that this `match!` is the same as the `if let` in `create_diagnostic`. I was trying to leave the general structure of the code as I found it, but some of this might be more clear without the separation into `create_diagnostic` too.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs`:196 on 2025-01-15 08:39_

I find the name `is_dep` slightly confusing because it explicitly excludes `Depends`. How about: `is_route_param` `is_route`

---

_@MichaReiser reviewed on 2025-01-15 08:56_

I spent some more time reading the code and I think I now have a better understanding what the fix is doing. 

I'd suggest the following changes:

* Change `is_fastapi_dependency` to return `Option<FastApiDependency>` where `FastApiDependency` is an enum. This removes the need to call `resolve_qualified_name` again when creating the fix. 
* I'd suggest moving the logic for extracting the `default_value` out of `create_fix` and move it into `fastapi_non_annotated_dependency`. 
* I think we can then move the logic for `seen_default` entirely into `fastapi_non_annotated_dependency`

<details><summary>Patch</summary>

```patch
Index: crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs b/crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs
--- a/crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs	(revision 6f7a8ad560f03638a7efe59c80a29a47da16003e)
+++ b/crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs	(date 1736931332112)
@@ -3,7 +3,7 @@
 use ruff_python_ast as ast;
 use ruff_python_ast::helpers::map_callable;
 use ruff_python_semantic::Modules;
-use ruff_text_size::Ranged;
+use ruff_text_size::{Ranged, TextRange};
 
 use crate::checkers::ast::Checker;
 use crate::importer::ImportRequest;
@@ -107,41 +107,94 @@
         .iter()
         .chain(&function_def.parameters.kwonlyargs)
     {
-        let needs_update = matches!(
-            (&parameter.parameter.annotation, &parameter.default),
-            (Some(_annotation), Some(default)) if is_fastapi_dependency(checker, default)
-        );
+        let (Some(annotation), Some(default)) =
+            (&parameter.parameter.annotation, &parameter.default)
+        else {
+            seen_default |= parameter.default.is_some();
+            continue;
+        };
 
-        if needs_update {
-            seen_default = create_diagnostic(checker, parameter, seen_default);
-        } else if parameter.default.is_some() {
-            seen_default = true;
+        if let Some(dependency) = is_fastapi_dependency(checker, default) {
+            let dependency_call = DependencyCall::from_expression(default);
+            let dependency_parameter = DependencyParameter {
+                annotation,
+                default,
+                kind: dependency,
+                name: &*parameter.parameter.name,
+                range: parameter.range,
+            };
+            seen_default =
+                create_diagnostic(checker, dependency_parameter, dependency_call, seen_default);
+        } else {
+            seen_default |= parameter.default.is_some();
         }
     }
 }
 
-fn is_fastapi_dependency(checker: &Checker, expr: &ast::Expr) -> bool {
+fn is_fastapi_dependency(checker: &Checker, expr: &ast::Expr) -> Option<FastApiDependency> {
     checker
         .semantic()
         .resolve_qualified_name(map_callable(expr))
-        .is_some_and(|qualified_name| {
-            matches!(
-                qualified_name.segments(),
-                [
-                    "fastapi",
-                    "Query"
-                        | "Path"
-                        | "Body"
-                        | "Cookie"
-                        | "Header"
-                        | "File"
-                        | "Form"
-                        | "Depends"
-                        | "Security"
-                ]
-            )
+        .and_then(|qualified_name| match qualified_name.segments() {
+            ["fastapi", dependency_name] => match *dependency_name {
+                "Query" => Some(FastApiDependency::Query),
+                "Path" => Some(FastApiDependency::Path),
+                "Body" => Some(FastApiDependency::Body),
+                "Cookie" => Some(FastApiDependency::Cookie),
+                "Header" => Some(FastApiDependency::Header),
+                "File" => Some(FastApiDependency::File),
+                "Form" => Some(FastApiDependency::Form),
+                "Depends" => Some(FastApiDependency::Depends),
+                "Security" => Some(FastApiDependency::Security),
+                _ => None,
+            },
+            _ => None,
         })
 }
+
+#[derive(Debug, Copy, Clone)]
+enum FastApiDependency {
+    Query,
+    Path,
+    Body,
+    Cookie,
+    Header,
+    File,
+    Form,
+    Depends,
+    Security,
+}
+
+struct DependencyParameter<'a> {
+    annotation: &'a ast::Expr,
+    default: &'a ast::Expr,
+    range: TextRange,
+    name: &'a str,
+    kind: FastApiDependency,
+}
+
+struct DependencyCall<'a> {
+    default_argument: ast::ArgOrKeyword<'a>,
+    keyword_arguments: Vec<&'a ast::Keyword>,
+}
+
+impl<'a> DependencyCall<'a> {
+    fn from_expression(expr: &'a ast::Expr) -> Option<Self> {
+        let call = expr.as_call_expr()?;
+        let default_argument = call.arguments.find_argument("default", 0)?;
+        let keyword_arguments = call
+            .arguments
+            .keywords
+            .iter()
+            .filter(|kwarg| kwarg.arg.as_ref().map_or(false, |name| name != "default"))
+            .collect();
+
+        Some(Self {
+            default_argument,
+            keyword_arguments,
+        })
+    }
+}
 
 /// Create a [`Diagnostic`] for `parameter` and return an updated value of `seen_default`.
 ///
@@ -164,7 +217,8 @@
 /// `seen_default` here.
 fn create_diagnostic(
     checker: &mut Checker,
-    parameter: &ast::ParameterWithDefault,
+    parameter: DependencyParameter,
+    dependency_call: Option<DependencyCall>,
     mut seen_default: bool,
 ) -> bool {
     let mut diagnostic = Diagnostic::new(
@@ -174,94 +228,78 @@
         parameter.range,
     );
 
-    if let (Some(annotation), Some(default)) = (&parameter.parameter.annotation, &parameter.default)
-    {
-        let mut try_generate_fix = || {
-            let module = if checker.settings.target_version >= PythonVersion::Py39 {
-                "typing"
-            } else {
-                "typing_extensions"
-            };
-            let (import_edit, binding) = checker.importer().get_or_import_symbol(
-                &ImportRequest::import_from(module, "Annotated"),
-                parameter.range.start(),
-                checker.semantic(),
-            )?;
+    let mut try_generate_fix = || {
+        let module = if checker.settings.target_version >= PythonVersion::Py39 {
+            "typing"
+        } else {
+            "typing_extensions"
+        };
+        let (import_edit, binding) = checker.importer().get_or_import_symbol(
+            &ImportRequest::import_from(module, "Annotated"),
+            parameter.range.start(),
+            checker.semantic(),
+        )?;
 
-            // Refine the match from `is_fastapi_dependency` to exclude Depends
-            // and Security, which don't have the same argument structure. The
-            // others need to be converted from `q: str = Query("")` to `q:
-            // Annotated[str, Query()] = ""` for example, but Depends and
-            // Security need to stay like `Annotated[str, Depends(callable)]`
-            let is_dep = checker
-                .semantic()
-                .resolve_qualified_name(map_callable(default))
-                .is_some_and(|qualified_name| {
-                    !matches!(
-                        qualified_name.segments(),
-                        ["fastapi", "Depends" | "Security"]
-                    )
-                });
+        // Each of these classes takes a single, optional default
+        // argument, followed by kw-only arguments
+
+        // Refine the match from `is_fastapi_dependency` to exclude Depends
+        // and Security, which don't have the same argument structure. The
+        // others need to be converted from `q: str = Query("")` to `q:
+        // Annotated[str, Query()] = ""` for example, but Depends and
+        // Security need to stay like `Annotated[str, Depends(callable)]`
+        let is_dep = !matches!(
+            parameter.kind,
+            FastApiDependency::Depends | FastApiDependency::Security
+        );
 
-            // Each of these classes takes a single, optional default
-            // argument, followed by kw-only arguments
-            let default_arg = default
-                .as_call_expr()
-                .and_then(|args| args.arguments.find_argument("default", 0));
-
-            let kwarg_list: Option<Vec<_>> = default.as_call_expr().map(|args| {
-                args.arguments
-                    .keywords
+        let content = match dependency_call {
+            Some(dependency_call) if is_dep => {
+                let kwarg_list = dependency_call
+                    .keyword_arguments
                     .iter()
-                    .filter_map(|kwarg| match kwarg.arg.as_ref() {
-                        None => None,
-                        Some(name) if name == "default" => None,
-                        Some(_) => Some(checker.locator().slice(kwarg.range())),
-                    })
-                    .collect()
-            });
+                    .map(|kwarg| checker.locator().slice(kwarg.range()))
+                    .collect::<Vec<_>>()
+                    .join(", ");
 
-            let content = match default_arg {
-                Some(default_arg) if is_dep => {
-                    let kwarg_list = match kwarg_list {
-                        Some(v) => v.join(", "),
-                        None => String::new(),
-                    };
-                    seen_default = true;
-                    format!(
-                        "{parameter_name}: {binding}[{annotation}, {default_}({kwarg_list})] \
+                seen_default = true;
+                format!(
+                    "{parameter_name}: {binding}[{annotation}, {default_}({kwarg_list})] \
                             = {default_value}",
-                        parameter_name = &parameter.parameter.name.id,
-                        annotation = checker.locator().slice(annotation.range()),
-                        default_ = checker.locator().slice(map_callable(default).range()),
-                        default_value = checker.locator().slice(default_arg.value().range()),
-                    )
-                }
-                _ => {
-                    if seen_default {
-                        return Ok(None);
-                    }
-                    format!(
-                        "{parameter_name}: {binding}[{annotation}, {default_}]",
-                        parameter_name = parameter.parameter.name.id,
-                        annotation = checker.locator().slice(annotation.range()),
-                        default_ = checker.locator().slice(default.range())
-                    )
-                }
-            };
-            let parameter_edit = Edit::range_replacement(content, parameter.range);
-            Ok(Some(Fix::unsafe_edits(import_edit, [parameter_edit])))
-        };
+                    parameter_name = parameter.name,
+                    annotation = checker.locator().slice(parameter.annotation.range()),
+                    default_ = checker
+                        .locator()
+                        .slice(map_callable(parameter.default).range()),
+                    default_value = checker
+                        .locator()
+                        .slice(dependency_call.default_argument.value().range()),
+                )
+            }
+            _ => {
+                if seen_default {
+                    return Ok(None);
+                }
+                format!(
+                    "{parameter_name}: {binding}[{annotation}, {default_}]",
+                    parameter_name = parameter.name,
+                    annotation = checker.locator().slice(parameter.annotation.range()),
+                    default_ = checker.locator().slice(parameter.default.range())
+                )
+            }
+        };
+        let parameter_edit = Edit::range_replacement(content, parameter.range);
+        Ok(Some(Fix::unsafe_edits(import_edit, [parameter_edit])))
+    };
 
-        // make sure we set `seen_default` if we bail out of `try_generate_fix` early. we could
-        // `match` on the result directly, but still calling `try_set_optional_fix` avoids
-        // duplicating the debug logging here
-        let fix: anyhow::Result<Option<Fix>> = try_generate_fix();
-        if fix.is_err() {
-            seen_default = true;
-        }
-        diagnostic.try_set_optional_fix(|| fix);
-    }
+    // make sure we set `seen_default` if we bail out of `try_generate_fix` early. we could
+    // `match` on the result directly, but still calling `try_set_optional_fix` avoids
+    // duplicating the debug logging here
+    let fix: anyhow::Result<Option<Fix>> = try_generate_fix();
+    if fix.is_err() {
+        seen_default = true;
+    }
+    diagnostic.try_set_optional_fix(|| fix);
 
     checker.diagnostics.push(diagnostic);
 
```
</details>

---

_Comment by @ntBre on 2025-01-15 15:54_

Wow thanks for the patch! I think it's much easier to follow now. I applied the patch, made a few clippy fixes, and then renamed `is_dep`. `is_route_param` makes a lot more sense.

---

_@MichaReiser approved on 2025-01-15 16:54_

---

_Merged by @ntBre on 2025-01-15 18:05_

---

_Closed by @ntBre on 2025-01-15 18:05_

---

_Branch deleted on 2025-01-15 18:05_

---
