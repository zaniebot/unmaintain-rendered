```yaml
number: 15654
title: "[`flake8-bandit`] Add missing single-line/dotall regex flag (`S608`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
assignees: []
merged: true
base: main
head: S608
created_at: 2025-01-21T19:32:02Z
updated_at: 2025-01-22T12:10:14Z
url: https://github.com/astral-sh/ruff/pull/15654
synced_at: 2026-01-12T15:55:52Z
```

# [`flake8-bandit`] Add missing single-line/dotall regex flag (`S608`)

---

_@InSyncWithFoo_

## Summary

Resolves #15653.

## Test Plan

`cargo nextest run` and `cargo insta test`.

---

_Comment by @github-actions[bot] on 2025-01-21 19:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+5 -0 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/4280b83977cd5a53c2b24143f3c9a6a63e298acc/providers/tests/integration/google/cloud/transfers/test_mssql_to_gcs.py#L58'>providers/tests/integration/google/cloud/transfers/test_mssql_to_gcs.py:58:13:</a> S608 Possible SQL injection vector through string-based query construction
+ <a href='https://github.com/apache/airflow/blob/4280b83977cd5a53c2b24143f3c9a6a63e298acc/providers/tests/system/google/cloud/dataflow/example_dataflow_sql.py#L97'>providers/tests/system/google/cloud/dataflow/example_dataflow_sql.py:97:15:</a> S608 Possible SQL injection vector through string-based query construction
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/b5ac415afc9a6b1d50d1aaa2f33146531e045024/superset/migrations/versions/2022-04-01_14-38_a9422eeaae74_new_dataset_models_take_2.py#L871'>superset/migrations/versions/2022-04-01_14-38_a9422eeaae74_new_dataset_models_take_2.py:871:9:</a> S608 Possible SQL injection vector through string-based query construction
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/dc23b9f574677c19a4c447f5c0226732c47be044/ibis/backends/mssql/__init__.py#L322'>ibis/backends/mssql/__init__.py:322:17:</a> S608 Possible SQL injection vector through string-based query construction
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/cb5286d743d4035deed5090e91136ca8c825da93/indico/migrations/versions/20210224_1808_26806768cd3f_remove_flower_oauth_app.py#L31'>indico/migrations/versions/20210224_1808_26806768cd3f_remove_flower_oauth_app.py:31:16:</a> S608 Possible SQL injection vector through string-based query construction
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S608 | 5 | 5 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+5 -0 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/4280b83977cd5a53c2b24143f3c9a6a63e298acc/providers/tests/integration/google/cloud/transfers/test_mssql_to_gcs.py#L58'>providers/tests/integration/google/cloud/transfers/test_mssql_to_gcs.py:58:13:</a> S608 Possible SQL injection vector through string-based query construction
+ <a href='https://github.com/apache/airflow/blob/4280b83977cd5a53c2b24143f3c9a6a63e298acc/providers/tests/system/google/cloud/dataflow/example_dataflow_sql.py#L97'>providers/tests/system/google/cloud/dataflow/example_dataflow_sql.py:97:15:</a> S608 Possible SQL injection vector through string-based query construction
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/b5ac415afc9a6b1d50d1aaa2f33146531e045024/superset/migrations/versions/2022-04-01_14-38_a9422eeaae74_new_dataset_models_take_2.py#L871'>superset/migrations/versions/2022-04-01_14-38_a9422eeaae74_new_dataset_models_take_2.py:871:9:</a> S608 Possible SQL injection vector through string-based query construction
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/dc23b9f574677c19a4c447f5c0226732c47be044/ibis/backends/mssql/__init__.py#L322'>ibis/backends/mssql/__init__.py:322:17:</a> S608 Possible SQL injection vector through string-based query construction
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/cb5286d743d4035deed5090e91136ca8c825da93/indico/migrations/versions/20210224_1808_26806768cd3f_remove_flower_oauth_app.py#L31'>indico/migrations/versions/20210224_1808_26806768cd3f_remove_flower_oauth_app.py:31:16:</a> S608 Possible SQL injection vector through string-based query construction
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S608 | 5 | 5 | 0 | 0 | 0 |

</p>
</details>




---

_Label `bug` added by @dhruvmanila on 2025-01-22 04:43_

---

_@dhruvmanila approved on 2025-01-22 04:48_

---

_Merged by @dhruvmanila on 2025-01-22 04:50_

---

_Closed by @dhruvmanila on 2025-01-22 04:50_

---

_Branch deleted on 2025-01-22 12:10_

---
