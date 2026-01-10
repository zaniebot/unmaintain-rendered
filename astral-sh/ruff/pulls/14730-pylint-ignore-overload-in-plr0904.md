```yaml
number: 14730
title: "[`pylint`] Ignore overload in `PLR0904`"
type: pull_request
state: merged
author: Matt-Ord
labels:
  - bug
assignees: []
merged: true
base: main
head: Ignore-overloads-in-too-many-public-methods
created_at: 2024-12-02T14:10:16Z
updated_at: 2024-12-02T14:38:30Z
url: https://github.com/astral-sh/ruff/pull/14730
synced_at: 2026-01-10T20:42:27Z
```

# [`pylint`] Ignore overload in `PLR0904`

---

_Pull request opened by @Matt-Ord on 2024-12-02 14:10_

Fixes #14727

## Summary

Fixes #14727

## Test Plan

cargo test


---

_Comment by @github-actions[bot] on 2024-12-02 14:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+6 -10 violations, +0 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/PlasmaPy/PlasmaPy/blob/fe54767fd5402f6f231cbe8fa88f9bfc9f8103e8/src/plasmapy/particles/particle_collections.py#L56'>src/plasmapy/particles/particle_collections.py:56:1:</a> PLR0904 Too many public methods (21 > 20)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/configuration.py#L184'>airflow/configuration.py:184:1:</a> PLR0904 Too many public methods (40 > 20)
- <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/configuration.py#L184'>airflow/configuration.py:184:1:</a> PLR0904 Too many public methods (42 > 20)
+ <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/models/dag.py#L307'>airflow/models/dag.py:307:1:</a> PLR0904 Too many public methods (40 > 20)
- <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/airflow/models/dag.py#L307'>airflow/models/dag.py:307:1:</a> PLR0904 Too many public methods (42 > 20)
+ <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/providers/src/airflow/providers/common/sql/hooks/sql.py#L102'>providers/src/airflow/providers/common/sql/hooks/sql.py:102:1:</a> PLR0904 Too many public methods (32 > 20)
- <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/providers/src/airflow/providers/common/sql/hooks/sql.py#L102'>providers/src/airflow/providers/common/sql/hooks/sql.py:102:1:</a> PLR0904 Too many public methods (34 > 20)
+ <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/providers/src/airflow/providers/google/cloud/hooks/gcs.py#L150'>providers/src/airflow/providers/google/cloud/hooks/gcs.py:150:1:</a> PLR0904 Too many public methods (27 > 20)
- <a href='https://github.com/apache/airflow/blob/3c1124e054f9d795b0e9007041653888da9de6a8/providers/src/airflow/providers/google/cloud/hooks/gcs.py#L150'>providers/src/airflow/providers/google/cloud/hooks/gcs.py:150:1:</a> PLR0904 Too many public methods (29 > 20)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L244'>src/bokeh/core/has_props.py:244:1:</a> PLR0904 Too many public methods (24 > 20)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/plots.py#L115'>src/bokeh/models/plots.py:115:1:</a> PLR0904 Too many public methods (21 > 20)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/0b99e4781940527a82749a235206b4044da6b8cb/ibis/expr/types/relations.py#L144'>ibis/expr/types/relations.py:144:1:</a> PLR0904 Too many public methods (61 > 20)
- <a href='https://github.com/ibis-project/ibis/blob/0b99e4781940527a82749a235206b4044da6b8cb/ibis/expr/types/relations.py#L144'>ibis/expr/types/relations.py:144:1:</a> PLR0904 Too many public methods (63 > 20)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/9d4f36d87dae9a968fb527e2cb87e8a507b0beb3/src/_pytest/_py/path.py#L265'>src/_pytest/_py/path.py:265:1:</a> PLR0904 Too many public methods (66 > 20)
- <a href='https://github.com/pytest-dev/pytest/blob/9d4f36d87dae9a968fb527e2cb87e8a507b0beb3/src/_pytest/_py/path.py#L265'>src/_pytest/_py/path.py:265:1:</a> PLR0904 Too many public methods (68 > 20)
- <a href='https://github.com/pytest-dev/pytest/blob/9d4f36d87dae9a968fb527e2cb87e8a507b0beb3/src/_pytest/nodes.py#L128'>src/_pytest/nodes.py:128:1:</a> PLR0904 Too many public methods (22 > 20)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0904 | 16 | 6 | 10 | 0 | 0 |

</p>
</details>




---

_Label `bug` added by @charliermarsh on 2024-12-02 14:21_

---

_Renamed from "Ignore overload in PLR0904" to "[`pylint`] Ignore overload in `PLR0904`" by @charliermarsh on 2024-12-02 14:32_

---

_@charliermarsh approved on 2024-12-02 14:33_

---

_Merged by @charliermarsh on 2024-12-02 14:36_

---

_Closed by @charliermarsh on 2024-12-02 14:36_

---

_Branch deleted on 2024-12-02 14:37_

---
