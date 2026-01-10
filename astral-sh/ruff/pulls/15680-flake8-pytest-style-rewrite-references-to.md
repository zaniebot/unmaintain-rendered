```yaml
number: 15680
title: "[`flake8-pytest-style`] Rewrite references to `.exception` (`PT027`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - fixes
assignees: []
merged: true
base: main
head: PT027
created_at: 2025-01-23T01:25:24Z
updated_at: 2025-01-23T16:53:24Z
url: https://github.com/astral-sh/ruff/pull/15680
synced_at: 2026-01-10T20:05:43Z
```

# [`flake8-pytest-style`] Rewrite references to `.exception` (`PT027`)

---

_Pull request opened by @InSyncWithFoo on 2025-01-23 01:25_

## Summary

Resolves #8650.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-23 01:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -2 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/6d117ffbb564f611df21fcb9a19c15b9567f35d9/tests/integration_tests/charts/schema_tests.py#L59'>tests/integration_tests/charts/schema_tests.py:59:9:</a> ANN201 Missing return type annotation for public function `test_query_context_series_limit`
- <a href='https://github.com/apache/superset/blob/6d117ffbb564f611df21fcb9a19c15b9567f35d9/tests/integration_tests/charts/schema_tests.py#L59'>tests/integration_tests/charts/schema_tests.py:59:9:</a> ANN201 Missing return type annotation for public function `test_query_context_series_limit`
+ <a href='https://github.com/apache/superset/blob/6d117ffbb564f611df21fcb9a19c15b9567f35d9/tests/integration_tests/viz_tests.py#L739'>tests/integration_tests/viz_tests.py:739:9:</a> ANN201 Missing return type annotation for public function `test_filter_nulls`
- <a href='https://github.com/apache/superset/blob/6d117ffbb564f611df21fcb9a19c15b9567f35d9/tests/integration_tests/viz_tests.py#L739'>tests/integration_tests/viz_tests.py:739:9:</a> ANN201 Missing return type annotation for public function `test_filter_nulls`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ANN201 | 4 | 2 | 2 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -3 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/6d117ffbb564f611df21fcb9a19c15b9567f35d9/tests/integration_tests/charts/schema_tests.py#L59'>tests/integration_tests/charts/schema_tests.py:59:9:</a> PLR6301 Method `test_query_context_series_limit` could be a function, class method, or static method
+ <a href='https://github.com/apache/superset/blob/6d117ffbb564f611df21fcb9a19c15b9567f35d9/tests/integration_tests/charts/schema_tests.py#L59'>tests/integration_tests/charts/schema_tests.py:59:9:</a> PLR6301 Method `test_query_context_series_limit` could be a function, class method, or static method
- <a href='https://github.com/apache/superset/blob/6d117ffbb564f611df21fcb9a19c15b9567f35d9/tests/integration_tests/model_tests.py#L202'>tests/integration_tests/model_tests.py:202:9:</a> PLR6301 Method `test_adjust_engine_params_mysql` could be a function, class method, or static method
+ <a href='https://github.com/apache/superset/blob/6d117ffbb564f611df21fcb9a19c15b9567f35d9/tests/integration_tests/model_tests.py#L202'>tests/integration_tests/model_tests.py:202:9:</a> PLR6301 Method `test_adjust_engine_params_mysql` could be a function, class method, or static method
+ <a href='https://github.com/apache/superset/blob/6d117ffbb564f611df21fcb9a19c15b9567f35d9/tests/integration_tests/viz_tests.py#L899'>tests/integration_tests/viz_tests.py:899:9:</a> ANN201 Missing return type annotation for public function `test_apply_rolling_without_data`
- <a href='https://github.com/apache/superset/blob/6d117ffbb564f611df21fcb9a19c15b9567f35d9/tests/integration_tests/viz_tests.py#L899'>tests/integration_tests/viz_tests.py:899:9:</a> ANN201 Missing return type annotation for public function `test_apply_rolling_without_data`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR6301 | 4 | 2 | 2 | 0 | 0 |
| ANN201 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/assertion.rs`:371 on 2025-01-23 07:08_

Do you mean `unittest_raises_assertion_binding`?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/assertion.rs`:276 on 2025-01-23 07:10_

It took me a while to realise that the changes here are entirely unrelated to the PR itself. It can be helpful for changes like this to either add a comment that you decided to refactor it but it's unrelated or make its own PR

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/assertion.rs`:375 on 2025-01-23 07:12_

Do I understand it correctly that what we want to test if `context_expr` is the `call`? If so, I suggest using `AnyNodeRef::ptr_eq`, it makes it more explicit rather than comparing ranges.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/assertion.rs`:395 on 2025-01-23 07:12_

Nit: I'd remove this, it's already covered by line 397

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/assertion.rs`:433 on 2025-01-23 07:14_

A short inline comment explaining what this block does would be helpful
```suggestion
    // Rewrite all references to `context_var.exception` to `context_var.value`
    // ```py
    // .. Example
    // ```
    for reference_id in binding.references() {
        let reference = semantic.reference(reference_id);
        let node_id = reference.expression_id()?;

        let mut ancestors = semantic.expressions(node_id).skip(1);

        let Expr::Attribute(ast::ExprAttribute { attr, .. }) = ancestors.next()? else {
            continue;
        };

        if attr.as_str() == "exception" {
            edits.push(Edit::range_replacement("value".to_string(), attr.range));
        }
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/assertion.rs`:425 on 2025-01-23 07:16_

Inline mainly helps with cross-crate calls and it should mainly be used for hot-functions (there's a trade off with increased code size). Inlining more code can also lead to some compiler optimizations to no longer be applied because the function becomes too large. Furthermore, the compiler already applies heuristics to decide when to inline a function. That's why `inline` should only be used where it matters, I don't think it matters here.
```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/assertion.rs`:446 on 2025-01-23 07:18_

`impl` can lead to monomorphization and we don't need the flexibility here. We always have a vec (or can easily create an empty one)
```suggestion
    extra_edits: Vec<Edit>,
```

---

_@MichaReiser approved on 2025-01-23 07:20_

Thanks. A few nit comments

---

_@InSyncWithFoo reviewed on 2025-01-23 13:53_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/assertion.rs`:276 on 2025-01-23 13:53_

Sorry. I should have said that only the first commit was intended to address the issue in question, like what I did [here](https://github.com/astral-sh/ruff/pull/15665#issuecomment-2606000323), but I forgot.

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/assertion.rs`:395 on 2025-01-23 13:55_

I think I added this as a micro-optimization.

---

_@InSyncWithFoo reviewed on 2025-01-23 13:55_

---

_Label `fixes` added by @MichaReiser on 2025-01-23 15:47_

---

_@MichaReiser approved on 2025-01-23 16:50_

---

_Merged by @MichaReiser on 2025-01-23 16:50_

---

_Closed by @MichaReiser on 2025-01-23 16:50_

---

_Branch deleted on 2025-01-23 16:53_

---
