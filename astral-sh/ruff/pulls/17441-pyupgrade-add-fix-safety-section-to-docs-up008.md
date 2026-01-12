```yaml
number: 17441
title: "[`pyupgrade`] Add fix safety section to docs (`UP008`, `UP022`)"
type: pull_request
state: merged
author: Kalmaegi
labels:
  - documentation
assignees: []
merged: true
base: main
head: doc_fix_safety_for_super_call_with_parameters_and_replace_stdout_stderr
created_at: 2025-04-17T02:37:32Z
updated_at: 2025-05-27T04:48:24Z
url: https://github.com/astral-sh/ruff/pull/17441
synced_at: 2026-01-12T15:56:02Z
```

# [`pyupgrade`] Add fix safety section to docs (`UP008`, `UP022`)

---

_@Kalmaegi_

## Summary

add fix safety section to replace_stdout_stderr and super_call_with_parameters, for #15584 
I checked the behavior and found that these two files could only potentially delete the appended comments, so I submitted them as a PR.




---

_Renamed from "add fix safety section to docs" to "add fix safety section to repeated_keys docs" by @Kalmaegi on 2025-04-17 08:38_

---

_Renamed from "add fix safety section to repeated_keys docs" to "add fix safety section to docs" by @Kalmaegi on 2025-04-17 08:39_

---

_Assigned to @ntBre by @ntBre on 2025-04-18 17:39_

---

_Label `documentation` added by @ntBre on 2025-04-18 17:40_

---

_@ntBre approved on 2025-04-18 17:45_

Thanks! I think these are the only reasons these are unsafe, and I tested them in the playground to show that they both definitely delete comments.

---

_Comment by @github-actions[bot] on 2025-04-18 17:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+8 -6 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/113528cbb206bbf44d04282861ed96a789bcbfbd/providers/cncf/kubernetes/src/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_utils.py#L141'>providers/cncf/kubernetes/src/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_utils.py:141:17:</a> PERF403 Use `dict.update` instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/113528cbb206bbf44d04282861ed96a789bcbfbd/providers/cncf/kubernetes/src/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_utils.py#L141'>providers/cncf/kubernetes/src/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_utils.py:141:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
- <a href='https://github.com/apache/airflow/blob/113528cbb206bbf44d04282861ed96a789bcbfbd/providers/exasol/src/airflow/providers/exasol/hooks/exasol.py#L70'>providers/exasol/src/airflow/providers/exasol/hooks/exasol.py:70:17:</a> PERF403 Use `dict.update` instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/113528cbb206bbf44d04282861ed96a789bcbfbd/providers/exasol/src/airflow/providers/exasol/hooks/exasol.py#L70'>providers/exasol/src/airflow/providers/exasol/hooks/exasol.py:70:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
- <a href='https://github.com/apache/airflow/blob/113528cbb206bbf44d04282861ed96a789bcbfbd/providers/mysql/src/airflow/providers/mysql/hooks/mysql.py#L191'>providers/mysql/src/airflow/providers/mysql/hooks/mysql.py:191:17:</a> PERF403 Use `dict.update` instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/113528cbb206bbf44d04282861ed96a789bcbfbd/providers/mysql/src/airflow/providers/mysql/hooks/mysql.py#L191'>providers/mysql/src/airflow/providers/mysql/hooks/mysql.py:191:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
- <a href='https://github.com/apache/airflow/blob/113528cbb206bbf44d04282861ed96a789bcbfbd/providers/postgres/src/airflow/providers/postgres/hooks/postgres.py#L171'>providers/postgres/src/airflow/providers/postgres/hooks/postgres.py:171:17:</a> PERF403 Use `dict.update` instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/113528cbb206bbf44d04282861ed96a789bcbfbd/providers/postgres/src/airflow/providers/postgres/hooks/postgres.py#L171'>providers/postgres/src/airflow/providers/postgres/hooks/postgres.py:171:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/b589d44dfb1081d0ff31e81b793ba08885fa579f/superset/migrations/versions/2021-04-12_12-38_fc3a3a8ff221_migrate_filter_sets_to_new_format.py#L122'>superset/migrations/versions/2021-04-12_12-38_fc3a3a8ff221_migrate_filter_sets_to_new_format.py:122:21:</a> PERF403 Use `dict.update` instead of a for-loop
+ <a href='https://github.com/apache/superset/blob/b589d44dfb1081d0ff31e81b793ba08885fa579f/superset/migrations/versions/2021-04-12_12-38_fc3a3a8ff221_migrate_filter_sets_to_new_format.py#L122'>superset/migrations/versions/2021-04-12_12-38_fc3a3a8ff221_migrate_filter_sets_to_new_format.py:122:21:</a> PERF403 Use a dictionary comprehension instead of a for-loop
- <a href='https://github.com/apache/superset/blob/b589d44dfb1081d0ff31e81b793ba08885fa579f/superset/migrations/versions/2022-04-01_14-38_a9422eeaae74_new_dataset_models_take_2.py#L795'>superset/migrations/versions/2022-04-01_14-38_a9422eeaae74_new_dataset_models_take_2.py:795:21:</a> PERF403 Use `dict.update` instead of a for-loop
+ <a href='https://github.com/apache/superset/blob/b589d44dfb1081d0ff31e81b793ba08885fa579f/superset/migrations/versions/2022-04-01_14-38_a9422eeaae74_new_dataset_models_take_2.py#L795'>superset/migrations/versions/2022-04-01_14-38_a9422eeaae74_new_dataset_models_take_2.py:795:21:</a> PERF403 Use a dictionary comprehension instead of a for-loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/5f354ca51f24cfb3e6a11c0c3367d0abf8676637/pandas/tests/arrays/sparse/test_accessor.py#L247'>pandas/tests/arrays/sparse/test_accessor.py:247:30:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/5f354ca51f24cfb3e6a11c0c3367d0abf8676637/pandas/tests/arrays/sparse/test_accessor.py#L96'>pandas/tests/arrays/sparse/test_accessor.py:96:50:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PERF403 | 12 | 6 | 6 | 0 | 0 |
| RUF043 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Renamed from "add fix safety section to docs" to "[`pyupgrade`] Add fix safety section to docs (`UP008`, `UP022`)" by @ntBre on 2025-04-18 17:47_

---

_Merged by @ntBre on 2025-04-18 17:48_

---

_Closed by @ntBre on 2025-04-18 17:48_

---

_Comment by @ML719 on 2025-05-27 04:48_

Chosing to myself for my boundary,no third party API , only me

---
