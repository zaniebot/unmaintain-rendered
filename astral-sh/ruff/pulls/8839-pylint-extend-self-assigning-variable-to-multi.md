```yaml
number: 8839
title: "[`pylint`] Extend `self-assigning-variable` to multi-target assignments"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/self-assign
created_at: 2023-11-25T18:27:13Z
updated_at: 2023-11-25T18:42:21Z
url: https://github.com/astral-sh/ruff/pull/8839
synced_at: 2026-01-10T23:40:55Z
```

# [`pylint`] Extend `self-assigning-variable` to multi-target assignments

---

_Pull request opened by @charliermarsh on 2023-11-25 18:27_

Closes https://github.com/astral-sh/ruff/issues/8667.

---

_Label `bug` added by @charliermarsh on 2023-11-25 18:27_

---

_Comment by @github-actions[bot] on 2023-11-25 18:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+5 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/177da9016bbedcfa49c08256fdaf2fb537b97d6c/tests/providers/common/sql/operators/test_sql.py#L237'>tests/providers/common/sql/operators/test_sql.py:237:9:</a> PLW0127 Self-assignment of variable `operator`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/a465beeaf13f0bae6db47adbe56082daddf34319/ibis/backends/pandas/tests/test_client.py#L31'>ibis/backends/pandas/tests/test_client.py:31:5:</a> PLW0127 Self-assignment of variable `test_data`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/24fdde62c6afc383baecd9a3aceada4dca804266/pandas/plotting/_matplotlib/tools.py#L104'>pandas/plotting/_matplotlib/tools.py:104:29:</a> PLW0127 Self-assignment of variable `ncols`
+ <a href='https://github.com/pandas-dev/pandas/blob/24fdde62c6afc383baecd9a3aceada4dca804266/pandas/plotting/_matplotlib/tools.py#L106'>pandas/plotting/_matplotlib/tools.py:106:22:</a> PLW0127 Self-assignment of variable `nrows`
+ <a href='https://github.com/pandas-dev/pandas/blob/24fdde62c6afc383baecd9a3aceada4dca804266/pandas/tests/indexes/multi/test_join.py#L264'>pandas/tests/indexes/multi/test_join.py:264:5:</a> PLW0127 Self-assignment of variable `midx`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0127 | 5 | 5 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+5 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/177da9016bbedcfa49c08256fdaf2fb537b97d6c/tests/providers/common/sql/operators/test_sql.py#L237'>tests/providers/common/sql/operators/test_sql.py:237:9:</a> PLW0127 Self-assignment of variable `operator`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/a465beeaf13f0bae6db47adbe56082daddf34319/ibis/backends/pandas/tests/test_client.py#L31'>ibis/backends/pandas/tests/test_client.py:31:5:</a> PLW0127 Self-assignment of variable `test_data`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/24fdde62c6afc383baecd9a3aceada4dca804266/pandas/plotting/_matplotlib/tools.py#L104'>pandas/plotting/_matplotlib/tools.py:104:29:</a> PLW0127 Self-assignment of variable `ncols`
+ <a href='https://github.com/pandas-dev/pandas/blob/24fdde62c6afc383baecd9a3aceada4dca804266/pandas/plotting/_matplotlib/tools.py#L106'>pandas/plotting/_matplotlib/tools.py:106:22:</a> PLW0127 Self-assignment of variable `nrows`
+ <a href='https://github.com/pandas-dev/pandas/blob/24fdde62c6afc383baecd9a3aceada4dca804266/pandas/tests/indexes/multi/test_join.py#L264'>pandas/tests/indexes/multi/test_join.py:264:5:</a> PLW0127 Self-assignment of variable `midx`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0127 | 5 | 5 | 0 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2023-11-25 18:42_

---

_Closed by @charliermarsh on 2023-11-25 18:42_

---

_Branch deleted on 2023-11-25 18:42_

---
