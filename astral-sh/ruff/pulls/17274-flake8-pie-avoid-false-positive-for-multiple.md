```yaml
number: 17274
title: "[`flake8-pie`] Avoid false positive for multiple assignment with `auto()` (`PIE796`)"
type: pull_request
state: merged
author: twentyone212121
labels: []
assignees: []
merged: true
base: main
head: fix-issue-16868
created_at: 2025-04-07T13:34:33Z
updated_at: 2025-04-08T19:53:28Z
url: https://github.com/astral-sh/ruff/pull/17274
synced_at: 2026-01-10T19:40:37Z
```

# [`flake8-pie`] Avoid false positive for multiple assignment with `auto()` (`PIE796`)

---

_Pull request opened by @twentyone212121 on 2025-04-07 13:34_

This fix closes #16868 

I noticed the issue is assigned, but the assignee appears to be actively working on another pull request. I hope that’s okay!

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

As of Python 3.11.1, `enum.auto()` can be used in multiple assignments. This pattern should not trigger non-unique-enums check.
Reference: [Python docs on enum.auto()](https://docs.python.org/3/library/enum.html#enum.auto)

This fix updates the check logic to skip enum variant statements where the right-hand side is a tuple containing a call to `enum.auto()`.

## Test Plan

<!-- How was it tested? -->

The added test case uses the example from the original issue. It previously triggered a false positive, but now passes successfully.


---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pie/rules/non_unique_enums.rs`:112 on 2025-04-07 21:25_

This is slightly neater, maybe:

```suggestion
    expr.as_call_expr().map_or(false, |call| {
        checker
            .semantic()
            .resolve_qualified_name(&call.func)
            .is_some_and(|qualified_name| matches!(qualified_name.segments(), ["enum", "auto"]))
    })
```

We could also consider passing only the `SemanticModel` into this function instead of the whole `Checker`, but I don't feel too strongly about that.

---

_@ntBre approved on 2025-04-07 21:29_

Thanks! A couple of stylistic suggestions, but this looks good to me.

---

_Comment by @github-actions[bot] on 2025-04-07 21:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -2 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/d5ea5890fd7a8b769935638c50a46214a553fc44/performance/src/performance_dags/performance_dag/performance_dag.py#L95'>performance/src/performance_dags/performance_dag/performance_dag.py:95:13:</a> AIR302 Import path `airflow.operators.bash` is moved into `standard` provider in Airflow 3.0;
- <a href='https://github.com/apache/airflow/blob/d5ea5890fd7a8b769935638c50a46214a553fc44/performance/src/performance_dags/performance_dag/performance_dag.py#L95'>performance/src/performance_dags/performance_dag/performance_dag.py:95:13:</a> AIR302 `airflow.operators.bash.BashOperator` is moved into `standard` provider in Airflow 3.0;
+ <a href='https://github.com/apache/airflow/blob/d5ea5890fd7a8b769935638c50a46214a553fc44/providers/edge/src/airflow/providers/edge/example_dags/integration_test.py#L116'>providers/edge/src/airflow/providers/edge/example_dags/integration_test.py:116:24:</a> AIR302 Import path `airflow.operators.bash` is moved into `standard` provider in Airflow 3.0;
- <a href='https://github.com/apache/airflow/blob/d5ea5890fd7a8b769935638c50a46214a553fc44/providers/edge/src/airflow/providers/edge/example_dags/integration_test.py#L116'>providers/edge/src/airflow/providers/edge/example_dags/integration_test.py:116:24:</a> AIR302 `airflow.operators.bash.BashOperator` is moved into `standard` provider in Airflow 3.0;
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR302 | 4 | 2 | 2 | 0 | 0 |

</p>
</details>




---

_@twentyone212121 reviewed on 2025-04-08 00:12_

---

_Review comment by @twentyone212121 on `crates/ruff_linter/src/rules/flake8_pie/rules/non_unique_enums.rs`:112 on 2025-04-08 00:12_

Thanks for the review! I updated the code in the latest commit. 
I also replaced `Checker` with `SemanticModel` in helper functions because:
1. They only need the `SemanticModel`.
2. We already have a `semantic` variable on the call site.
3. Since `Checker` can be mutated through shared references, keeping its usage minimal felt like a safer option.

---

_@ntBre approved on 2025-04-08 19:17_

Very nice, thanks!

---

_Renamed from "[`flake8-pie`] Avoid false positive for multiple assignment with auto() (`PIE796`)" to "[`flake8-pie`] Avoid false positive for multiple assignment with `auto()` (`PIE796`)" by @ntBre on 2025-04-08 19:53_

---

_Merged by @ntBre on 2025-04-08 19:53_

---

_Closed by @ntBre on 2025-04-08 19:53_

---
