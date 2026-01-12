```yaml
number: 8572
title: Detect runtime-evaluated base classes defined in the current file
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: evanrittenhouse/runtime_eval_base_class
created_at: 2023-11-09T03:16:36Z
updated_at: 2023-11-09T03:38:08Z
url: https://github.com/astral-sh/ruff/pull/8572
synced_at: 2026-01-10T23:40:55Z
```

# Detect runtime-evaluated base classes defined in the current file

---

_Pull request opened by @charliermarsh on 2023-11-09 03:16_

Closes https://github.com/astral-sh/ruff/issues/8250.

Closes https://github.com/astral-sh/ruff/issues/5486.


---

_Comment by @github-actions[bot] on 2023-11-09 03:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/example_dags/example_dynamic_task_mapping_with_no_taskflow_operators.py#L62'>airflow/example_dags/example_dynamic_task_mapping_with_no_taskflow_operators.py:62:5:</a> AIR001 Task variable name should match the `task_id`: "sum_it"
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR001 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/example_dags/example_dynamic_task_mapping_with_no_taskflow_operators.py#L62'>airflow/example_dags/example_dynamic_task_mapping_with_no_taskflow_operators.py:62:5:</a> AIR001 Task variable name should match the `task_id`: "sum_it"
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR001 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2023-11-09 03:38_

---

_Closed by @charliermarsh on 2023-11-09 03:38_

---

_Branch deleted on 2023-11-09 03:38_

---
