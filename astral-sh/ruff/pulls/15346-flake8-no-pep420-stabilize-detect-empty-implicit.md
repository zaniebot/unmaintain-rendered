```yaml
number: 15346
title: "[`flake8-no-pep420`] Stabilize: Detect empty implicit namespace packages (`INP001`)"
type: pull_request
state: closed
author: MichaReiser
labels: []
assignees: []
base: ruff-0.9
head: micha/implicit-namespace-packages
created_at: 2025-01-08T11:58:11Z
updated_at: 2025-01-08T12:07:27Z
url: https://github.com/astral-sh/ruff/pull/15346
synced_at: 2026-01-10T20:34:00Z
```

# [`flake8-no-pep420`] Stabilize: Detect empty implicit namespace packages (`INP001`)

---

_Pull request opened by @MichaReiser on 2025-01-08 11:58_

## Summary

I reconsidered and I think we shouldn't stabilize this today, considering that we already know that it isn't working in the LSP https://github.com/astral-sh/ruff/issues/14752

## Test Plan

<!-- How was it tested? -->


---

_Closed by @MichaReiser on 2025-01-08 11:59_

---

_Comment by @github-actions[bot] on 2025-01-08 12:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+17 -6 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+10 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/lineage/entities.py#L64'>airflow/lineage/entities.py:64:23:</a> RUF008 Do not use mutable default values for dataclass attributes
- <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/lineage/entities.py#L64'>airflow/lineage/entities.py:64:23:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/lineage/entities.py#L86'>airflow/lineage/entities.py:86:23:</a> RUF008 Do not use mutable default values for dataclass attributes
- <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/lineage/entities.py#L86'>airflow/lineage/entities.py:86:23:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/lineage/entities.py#L88'>airflow/lineage/entities.py:88:29:</a> RUF008 Do not use mutable default values for dataclass attributes
- <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/lineage/entities.py#L88'>airflow/lineage/entities.py:88:29:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/lineage/entities.py#L89'>airflow/lineage/entities.py:89:26:</a> RUF008 Do not use mutable default values for dataclass attributes
- <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/lineage/entities.py#L89'>airflow/lineage/entities.py:89:26:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/lineage/entities.py#L90'>airflow/lineage/entities.py:90:29:</a> RUF008 Do not use mutable default values for dataclass attributes
- <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/lineage/entities.py#L90'>airflow/lineage/entities.py:90:29:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/models/dag.py#L430'>airflow/models/dag.py:430:25:</a> RUF009 Do not perform function call in dataclass defaults
+ <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/models/dag.py#L431'>airflow/models/dag.py:431:24:</a> RUF009 Do not perform function call `airflow_conf.get_mandatory_value` in dataclass defaults
+ <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/providers/src/airflow/__init__.py#L1'>providers/src/airflow/__init__.py:1:1:</a> INP001 File `providers/src/airflow/__init__.py` declares a package, but is nested under an implicit namespace package. Add an `__init__.py` to `providers/src`.
+ <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/providers/src/airflow/providers/papermill/operators/papermill.py#L42'>providers/src/airflow/providers/papermill/operators/papermill.py:42:31:</a> RUF008 Do not use mutable default values for dataclass attributes
- <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/providers/src/airflow/providers/papermill/operators/papermill.py#L42'>providers/src/airflow/providers/papermill/operators/papermill.py:42:31:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/task_sdk/src/airflow/__init__.py#L1'>task_sdk/src/airflow/__init__.py:1:1:</a> INP001 File `task_sdk/src/airflow/__init__.py` declares a package, but is nested under an implicit namespace package. Add an `__init__.py` to `task_sdk/src`.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/embed/ext_package_no_main/__init__.py#L1'>tests/unit/bokeh/embed/ext_package_no_main/__init__.py:1:1:</a> INP001 File `tests/unit/bokeh/embed/ext_package_no_main/__init__.py` declares a package, but is nested under an implicit namespace package. Add an `__init__.py` to `tests/unit/bokeh/embed`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/embed/latex_label/__init__.py#L1'>tests/unit/bokeh/embed/latex_label/__init__.py:1:1:</a> INP001 File `tests/unit/bokeh/embed/latex_label/__init__.py` declares a package, but is nested under an implicit namespace package. Add an `__init__.py` to `tests/unit/bokeh/embed`.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/5d877d54d09a8ae016de1f46f43cc2528ff08a89/tests/units/test_attribute_access_type.py#L303'>tests/units/test_attribute_access_type.py:303:27:</a> RUF008 Do not use mutable default values for dataclass attributes
+ <a href='https://github.com/reflex-dev/reflex/blob/5d877d54d09a8ae016de1f46f43cc2528ff08a89/tests/units/test_attribute_access_type.py#L304'>tests/units/test_attribute_access_type.py:304:27:</a> RUF008 Do not use mutable default values for dataclass attributes
+ <a href='https://github.com/reflex-dev/reflex/blob/5d877d54d09a8ae016de1f46f43cc2528ff08a89/tests/units/test_attribute_access_type.py#L307'>tests/units/test_attribute_access_type.py:307:31:</a> RUF008 Do not use mutable default values for dataclass attributes
+ <a href='https://github.com/reflex-dev/reflex/blob/5d877d54d09a8ae016de1f46f43cc2528ff08a89/tests/units/test_attribute_access_type.py#L308'>tests/units/test_attribute_access_type.py:308:36:</a> RUF008 Do not use mutable default values for dataclass attributes
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/ffa796eb9a00954705fd4c59dada3b94788be6e8/astropy/utils/tests/data/test_package/__init__.py#L1'>astropy/utils/tests/data/test_package/__init__.py:1:1:</a> INP001 File `astropy/utils/tests/data/test_package/__init__.py` declares a package, but is nested under an implicit namespace package. Add an `__init__.py` to `astropy/utils/tests/data`.
</pre>

</p>
</details>
<details><summary>Changes by rule (4 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF008 | 10 | 10 | 0 | 0 | 0 |
| RUF012 | 6 | 0 | 6 | 0 | 0 |
| INP001 | 5 | 5 | 0 | 0 | 0 |
| RUF009 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Branch deleted on 2025-01-08 12:07_

---
