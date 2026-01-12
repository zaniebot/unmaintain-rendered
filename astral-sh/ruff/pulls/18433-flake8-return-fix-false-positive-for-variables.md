```yaml
number: 18433
title: " [`flake8-return`] Fix false-positive for variables used inside nested functions in `RET504` "
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
assignees: []
merged: true
base: main
head: RET504-false-positive
created_at: 2025-06-02T19:39:53Z
updated_at: 2025-07-10T20:11:19Z
url: https://github.com/astral-sh/ruff/pull/18433
synced_at: 2026-01-12T15:56:18Z
```

#  [`flake8-return`] Fix false-positive for variables used inside nested functions in `RET504` 

---

_@LaBatata101_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This PR is the same as #17656.

 I accidentally deleted the branch of that PR, so I'm creating a new one.

Fixes #14052

## Test Plan

Add regression tests
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-06-02 19:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+7 -7 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+6 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/f6f95821863536654c73553db02cdd03827c8113/scripts/erd/erd.py#L195'>scripts/erd/erd.py:195:5:</a> D413 [*] Missing blank line after last section ("Parameters")
- <a href='https://github.com/apache/superset/blob/f6f95821863536654c73553db02cdd03827c8113/scripts/erd/erd.py#L195'>scripts/erd/erd.py:195:5:</a> D413 [*] Missing blank line after last section ("Parameters")
+ <a href='https://github.com/apache/superset/blob/f6f95821863536654c73553db02cdd03827c8113/superset/sql_lab.py#L621'>superset/sql_lab.py:621:5:</a> D202 [*] No blank lines allowed after function docstring (found 1)
- <a href='https://github.com/apache/superset/blob/f6f95821863536654c73553db02cdd03827c8113/superset/sql_lab.py#L621'>superset/sql_lab.py:621:5:</a> D202 [*] No blank lines allowed after function docstring (found 1)
+ <a href='https://github.com/apache/superset/blob/f6f95821863536654c73553db02cdd03827c8113/tests/integration_tests/base_tests.py#L553'>tests/integration_tests/base_tests.py:553:9:</a> PLR0913 Too many arguments in function definition (12 > 5)
- <a href='https://github.com/apache/superset/blob/f6f95821863536654c73553db02cdd03827c8113/tests/integration_tests/base_tests.py#L553'>tests/integration_tests/base_tests.py:553:9:</a> PLR0913 Too many arguments in function definition (12 > 5)
+ <a href='https://github.com/apache/superset/blob/f6f95821863536654c73553db02cdd03827c8113/tests/integration_tests/core_tests.py#L873'>tests/integration_tests/core_tests.py:873:9:</a> D102 Missing docstring in public method
- <a href='https://github.com/apache/superset/blob/f6f95821863536654c73553db02cdd03827c8113/tests/integration_tests/core_tests.py#L873'>tests/integration_tests/core_tests.py:873:9:</a> D102 Missing docstring in public method
+ <a href='https://github.com/apache/superset/blob/f6f95821863536654c73553db02cdd03827c8113/tests/integration_tests/reports/utils.py#L194'>tests/integration_tests/reports/utils.py:194:5:</a> ANN201 Missing return type annotation for public function `create_dashboard_report`
- <a href='https://github.com/apache/superset/blob/f6f95821863536654c73553db02cdd03827c8113/tests/integration_tests/reports/utils.py#L194'>tests/integration_tests/reports/utils.py:194:5:</a> ANN201 Missing return type annotation for public function `create_dashboard_report`
+ <a href='https://github.com/apache/superset/blob/f6f95821863536654c73553db02cdd03827c8113/tests/unit_tests/fixtures/bash_mock.py#L23'>tests/unit_tests/fixtures/bash_mock.py:23:9:</a> ANN205 Missing return type annotation for staticmethod `tag_latest_release`
- <a href='https://github.com/apache/superset/blob/f6f95821863536654c73553db02cdd03827c8113/tests/unit_tests/fixtures/bash_mock.py#L23'>tests/unit_tests/fixtures/bash_mock.py:23:9:</a> ANN205 Missing return type annotation for staticmethod `tag_latest_release`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/4d081c61244c76ed19137c96c65fd796306ea794/zerver/lib/query_helpers.py#L15'>zerver/lib/query_helpers.py:15:5:</a> D404 First word of the docstring should not be "This"
+ <a href='https://github.com/zulip/zulip/blob/4d081c61244c76ed19137c96c65fd796306ea794/zerver/lib/query_helpers.py#L15'>zerver/lib/query_helpers.py:15:5:</a> D404 First word of the docstring should not be "This"
</pre>

</p>
</details>
<details><summary>Changes by rule (7 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| D413 | 2 | 1 | 1 | 0 | 0 |
| D202 | 2 | 1 | 1 | 0 | 0 |
| PLR0913 | 2 | 1 | 1 | 0 | 0 |
| D102 | 2 | 1 | 1 | 0 | 0 |
| ANN201 | 2 | 1 | 1 | 0 | 0 |
| ANN205 | 2 | 1 | 1 | 0 | 0 |
| D404 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-06-02 19:53_

---

_Review requested from @ntBre by @MichaReiser on 2025-06-12 07:01_

---

_Comment by @ntBre on 2025-06-16 18:03_

I should have time to review this this week. Could you resolve the merge conflicts?

---

_Comment by @LaBatata101 on 2025-06-16 20:11_

> I should have time to review this this week. Could you resolve the merge conflicts?

Done!

---

_Comment by @ntBre on 2025-06-16 21:22_

I'll take a closer look at the implementation too, but I think there might be two new false negatives in the ecosystem check:
- https://github.com/zulip/zulip/blob/053f30aacecbdc83c38ebcb04745ea9a7c669243/zerver/lib/generate_test_data.py#L16
- https://github.com/apache/airflow/blob/d12cd80f0d9f007aea9050cbf317b7af05a78274/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L464

The second one especially looks like a pretty clear example of what the rule is meant to catch, but maybe I'm missing something here.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:614 on 2025-06-16 21:35_

I'm not sure about this, but I suspect that this is the cause of the new false negatives. Could this be checking other scopes not nested *within* the current function? Both of the false negative cases have instances of the same variable names in other functions in the same file. I don't know of an API off the top of my head, but we may need to restrict the search to children of the current scope.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:764 on 2025-06-16 21:37_

nit: I'd personally just return early in these cases (referring to this one and the new `unreachable!()` above instead of panicking. If you really want to check them but still avoid panicking in release builds, you could either add a `debug_assert!(false, "<message>")` or a `log::debug!` to print a message in `--verbose` mode, at least.

---

_Comment by @LaBatata101 on 2025-06-16 21:39_

> I'll take a closer look at the implementation too, but I think there might be two new false negatives in the ecosystem check:
> 
>     * https://github.com/zulip/zulip/blob/053f30aacecbdc83c38ebcb04745ea9a7c669243/zerver/lib/generate_test_data.py#L16
> 
>     * https://github.com/apache/airflow/blob/d12cd80f0d9f007aea9050cbf317b7af05a78274/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L464
> 
> 
> The second one especially looks like a pretty clear example of what the rule is meant to catch, but maybe I'm missing something here.

I've tested here, and it seems to be working. Unless there are other parts of the code causing some interference.
```python
def get_dag_prefix(performance_dag_conf: dict[str, str]) -> str:
    """
    Return DAG prefix.

    Returns prefix that will be assigned to DAGs created with given performance DAG configuration.

    :param performance_dag_conf: dict with environment variables as keys and their values as values

    :return: final form of prefix after substituting inappropriate characters
    :rtype: str
    """
    dag_prefix = get_performance_dag_environment_variable(performance_dag_conf, "PERF_DAG_PREFIX")

    safe_dag_prefix = safe_dag_id(dag_prefix)

    return safe_dag_prefix


def load_config() -> dict[str, Any]:
    with open("zerver/tests/fixtures/config.generate_data.json", "rb") as infile:
        config = orjson.loads(infile.read())

    return config

```
```shell
$ cargo run -p ruff -- check sample2.py --preview --no-cache --select RET504 

sample2.py:16:12: RET504 Unnecessary assignment to `safe_dag_prefix` before `return` statement
   |
14 |     safe_dag_prefix = safe_dag_id(dag_prefix)
15 |
16 |     return safe_dag_prefix
   |            ^^^^^^^^^^^^^^^ RET504
   |
   = help: Remove unnecessary assignment

sample2.py:23:12: RET504 Unnecessary assignment to `config` before `return` statement
   |
21 |         config = orjson.loads(infile.read())
22 |
23 |     return config
   |            ^^^^^^ RET504
   |
   = help: Remove unnecessary assignment

Found 2 errors.
No fixes available (2 hidden fixes can be enabled with the `--unsafe-fixes` option).
```

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_return/RET504.py`:423 on 2025-06-16 21:39_

Did we need to delete (and edit) the old tests here? The answer could be "yes" but they don't obviously overlap with the new tests, at least to me, so I'd lean towards keeping them.

---

_@ntBre reviewed on 2025-06-16 21:40_

---

_@ntBre reviewed on 2025-06-16 21:41_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:614 on 2025-06-16 21:41_

This might actually be corroborated by your comment. I think other functions in the file might be leaking into the current check, but I could still be wrong.

---

_@LaBatata101 reviewed on 2025-06-16 21:43_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/resources/test/fixtures/flake8_return/RET504.py`:423 on 2025-06-16 21:43_

I probably deleted by accident when solving the merge conflicts

---

_@LaBatata101 reviewed on 2025-06-16 22:17_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:614 on 2025-06-16 22:17_

I think the error is in the code above, where the search for `assigned_binding` is done.

---

_@LaBatata101 reviewed on 2025-06-16 22:40_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:614 on 2025-06-16 22:40_

I've come up with this solution where it tries to find the scope of the function by looking up its name. Then it looks up `assigned_binding` in the function scope. It solves the false negatives, but the only problem is that it doesn't work if we have more than one function with the same name (the test file has multiple functions with the same name). I've run out of ideas for how to solve this problem.
```diff
diff --git a/crates/ruff_linter/src/rules/flake8_return/rules/function.rs b/crates/ruff_linter/src/rules/flake8_return/rules/function.rs
index 299acbc6c..29d759dcd 100644
--- a/crates/ruff_linter/src/rules/flake8_return/rules/function.rs
+++ b/crates/ruff_linter/src/rules/flake8_return/rules/function.rs
@@ -7,8 +7,8 @@ use ruff_python_ast::visitor::Visitor;
 use ruff_python_ast::whitespace::indentation;
 use ruff_python_ast::{self as ast, Decorator, ElifElseClause, Expr, Stmt};
 use ruff_python_parser::TokenKind;
-use ruff_python_semantic::SemanticModel;
 use ruff_python_semantic::analyze::visibility::is_property;
+use ruff_python_semantic::{BindingKind, SemanticModel};
 use ruff_python_trivia::{SimpleTokenKind, SimpleTokenizer, is_python_whitespace};
 use ruff_source_file::LineRanges;
 use ruff_text_size::{Ranged, TextRange, TextSize};
@@ -568,6 +568,19 @@ pub(crate) fn unnecessary_assign(checker: &Checker, function_stmt: &Stmt) {
         return;
     }
 
+    let Some(function_scope) = checker
+        .semantic()
+        .lookup_symbol(&function_def.name)
+        .map(|binding_id| checker.semantic().binding(binding_id))
+        .and_then(|binding| {
+            let BindingKind::FunctionDefinition(scope_id) = &binding.kind else {
+                return None;
+            };
+            Some(&checker.semantic().scopes[*scope_id])
+        })
+    else {
+        return;
+    };
     for (assign, return_, stmt) in &stack.assignment_return {
         // Identify, e.g., `return x`.
         let Some(value) = return_.value.as_ref() else {
@@ -611,12 +624,9 @@ pub(crate) fn unnecessary_assign(checker: &Checker, function_stmt: &Stmt) {
             continue;
         }
 
-        let Some(assigned_binding) = checker
-            .semantic()
-            .bindings
-            .iter()
-            .filter(|binding| binding.kind.is_assignment())
-            .find(|binding| binding.name(checker.source()) == assigned_id.as_str())
+        let Some(assigned_binding) = function_scope
+            .get(assigned_id)
+            .map(|binding_id| checker.semantic().binding(binding_id))
         else {
             continue;
         };
```

---

_@ntBre reviewed on 2025-06-24 18:35_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:614 on 2025-06-24 18:35_

I'm not sure how helpful this is, but I spent some time minimizing the ecosystem hit that was causing problems:

```python
# minimized from an ecosystem hit in
# https://github.com/astral-sh/ruff/pull/18433#issuecomment-2932216413
def foo():
    safe_dag_prefix = 1
    [safe_dag_prefix for _ in range(5)]


def bar():
    safe_dag_prefix = 2
    return safe_dag_prefix  # Supposed to trigger RET504
```

It seems that it might have something to do with the shared variable name appearing in a list comprehension. The Airflow example has it in a position like `[... for x in ... if some_condition(safe_dag_prefix)]` but it happens with the variable in the comprehension element position too.

I think you're on the right track with looking up the scope. Is there not a way to look them up by something other than their name? `SemanticModel::current_scope` sounds promising, but maybe it doesn't work for deferred checks like function bodies. It feels like there should be a way to get the scope from `function_stmt`, but I could be wrong.

---

_Comment by @LaBatata101 on 2025-06-24 21:02_

@ntBre I think it's fixed now

---

_Review requested from @ntBre by @LaBatata101 on 2025-06-24 21:02_

---

_@ntBre approved on 2025-07-10 20:05_

Thanks

---

_Merged by @ntBre on 2025-07-10 20:10_

---

_Closed by @ntBre on 2025-07-10 20:10_

---

_Branch deleted on 2025-07-10 20:11_

---
