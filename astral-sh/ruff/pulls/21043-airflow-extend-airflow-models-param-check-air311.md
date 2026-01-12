```yaml
number: 21043
title: "[`airflow`] Extend `airflow.models..Param` check (`AIR311`)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
assignees: []
merged: true
base: main
head: extend-AIR311
created_at: 2025-10-23T10:32:36Z
updated_at: 2025-10-23T21:12:52Z
url: https://github.com/astral-sh/ruff/pull/21043
synced_at: 2026-01-12T15:57:15Z
```

# [`airflow`] Extend `airflow.models..Param` check (`AIR311`)

---

_@Lee-W_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

* Extend `airflow.models.Param` to include `airflow.models.param.Param` case and include both `airflow.models.param.ParamDict` and `airflow.models.param.DagParam` and their `airflow.models.` counter part

## Test Plan

<!-- How was it tested? -->

update the text fixture accordingly and reorganize them in the third commit


---

_Comment by @github-actions[bot] on 2025-10-23 10:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/080eed68466ba458d7c1e9d39163fc499050de32/providers/edge3/src/airflow/providers/edge3/example_dags/integration_test.py#L65'>providers/edge3/src/airflow/providers/edge3/example_dags/integration_test.py:65:26:</a> AIR311 `airflow.models.param.Param` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/080eed68466ba458d7c1e9d39163fc499050de32/providers/edge3/src/airflow/providers/edge3/example_dags/integration_test.py#L73'>providers/edge3/src/airflow/providers/edge3/example_dags/integration_test.py:73:27:</a> AIR311 `airflow.models.param.Param` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR311 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/080eed68466ba458d7c1e9d39163fc499050de32/providers/edge3/src/airflow/providers/edge3/example_dags/integration_test.py#L65'>providers/edge3/src/airflow/providers/edge3/example_dags/integration_test.py:65:26:</a> AIR311 `airflow.models.param.Param` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/080eed68466ba458d7c1e9d39163fc499050de32/providers/edge3/src/airflow/providers/edge3/example_dags/integration_test.py#L73'>providers/edge3/src/airflow/providers/edge3/example_dags/integration_test.py:73:27:</a> AIR311 `airflow.models.param.Param` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR311 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Renamed from "[`airflow`] Extend airflow.sdk.definitions.param.Param replacement to all `airflow.models..Param` (`AIR311`)" to "[`airflow`] Extend `airflow.models..Param` check (`AIR311`)" by @Lee-W on 2025-10-23 10:45_

---

_@zach-overflow reviewed on 2025-10-23 12:14_

Thank you!

---

_Review requested from @ntBre by @ntBre on 2025-10-23 18:17_

---

_Label `rule` added by @ntBre on 2025-10-23 18:17_

---

_@ntBre approved on 2025-10-23 21:12_

Thank you!

---

_Merged by @ntBre on 2025-10-23 21:12_

---

_Closed by @ntBre on 2025-10-23 21:12_

---
