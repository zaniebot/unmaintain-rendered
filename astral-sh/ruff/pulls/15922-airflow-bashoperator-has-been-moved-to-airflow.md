```yaml
number: 15922
title: "[airflow] `BashOperator` has been moved to `airflow.providers.standard.operators.bash.BashOperator` (AIR302)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: extend-AIR302
created_at: 2025-02-04T07:19:44Z
updated_at: 2025-02-04T08:58:00Z
url: https://github.com/astral-sh/ruff/pull/15922
synced_at: 2026-01-10T19:57:22Z
```

# [airflow] `BashOperator` has been moved to `airflow.providers.standard.operators.bash.BashOperator` (AIR302)

---

_Pull request opened by @Lee-W on 2025-02-04 07:19_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Extend AIR302 with 

* `airflow.operators.bash.BashOperator → airflow.providers.standard.operators.bash.BashOperator`
* change existing rules `airflow.operators.bash_operator.BashOperator → airflow.operators.bash.BashOperator` to `airflow.operators.bash_operator.BashOperator → airflow.providers.standard.operators.bash.BashOperator`

## Test Plan

<!-- How was it tested? -->
a test fixture has been updated


---

_Review requested from @carljm by @Lee-W on 2025-02-04 07:19_

---

_Review requested from @MichaReiser by @Lee-W on 2025-02-04 07:19_

---

_Review requested from @AlexWaygood by @Lee-W on 2025-02-04 07:19_

---

_Review requested from @sharkdp by @Lee-W on 2025-02-04 07:19_

---

_Review requested from @dhruvmanila by @Lee-W on 2025-02-04 07:19_

---

_Renamed from "Extend air302" to "[airflow] `BashOperator` has been moved to `airflow.providers.standard.operators.bash.BashOperator` (AIR302)" by @Lee-W on 2025-02-04 07:26_

---

_Closed by @Lee-W on 2025-02-04 07:28_

---

_Reopened by @Lee-W on 2025-02-04 07:28_

---

_Comment by @github-actions[bot] on 2025-02-04 07:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/e6ea6709bbd8ba7c024c4f75136a0af0cf9987b0/performance/src/performance_dags/performance_dag/performance_dag.py#L95'>performance/src/performance_dags/performance_dag/performance_dag.py:95:13:</a> AIR302 `airflow.operators.bash.BashOperator` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/e6ea6709bbd8ba7c024c4f75136a0af0cf9987b0/providers/edge/src/airflow/providers/edge/example_dags/integration_test.py#L115'>providers/edge/src/airflow/providers/edge/example_dags/integration_test.py:115:24:</a> AIR302 `airflow.operators.bash.BashOperator` is removed in Airflow 3.0
+ <a href='https://github.com/apache/airflow/blob/e6ea6709bbd8ba7c024c4f75136a0af0cf9987b0/tests/decorators/test_bash.py#L521'>tests/decorators/test_bash.py:521:13:</a> AIR302 `airflow.operators.bash.BashOperator` is removed in Airflow 3.0
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR302 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>




---

_Review request for @carljm removed by @AlexWaygood on 2025-02-04 07:58_

---

_Review request for @MichaReiser removed by @AlexWaygood on 2025-02-04 07:58_

---

_Review request for @sharkdp removed by @AlexWaygood on 2025-02-04 07:58_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-02-04 07:58_

---

_Review request for @dhruvmanila removed by @AlexWaygood on 2025-02-04 07:58_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:688 on 2025-02-04 08:14_

nit: we can merge the two match arms together:
```rs
["airflow", "operators", "bashr" | "bash_operator", "BashOperator"] => {
	Replacement::Name("airflow.providers.standard.operators.bash.BashOperator")
}
```

---

_@dhruvmanila approved on 2025-02-04 08:15_

---

_Label `rule` added by @dhruvmanila on 2025-02-04 08:15_

---

_Label `preview` added by @dhruvmanila on 2025-02-04 08:15_

---

_@Lee-W reviewed on 2025-02-04 08:17_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:688 on 2025-02-04 08:17_

ah right, just updated!

---

_Merged by @dhruvmanila on 2025-02-04 08:58_

---

_Closed by @dhruvmanila on 2025-02-04 08:58_

---
