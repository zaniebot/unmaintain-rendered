```yaml
number: 9815
title: Run dunder method rule on methods directly
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/dunder
created_at: 2024-02-04T19:13:02Z
updated_at: 2024-02-04T19:26:21Z
url: https://github.com/astral-sh/ruff/pull/9815
synced_at: 2026-01-10T22:57:09Z
```

# Run dunder method rule on methods directly

---

_Pull request opened by @charliermarsh on 2024-02-04 19:13_

This stood out in the flamegraph and I realized it requires us to traverse over all statements in the class (unnecessarily).

---

_Marked ready for review by @charliermarsh on 2024-02-04 19:13_

---

_Label `internal` added by @charliermarsh on 2024-02-04 19:13_

---

_Merged by @charliermarsh on 2024-02-04 19:24_

---

_Closed by @charliermarsh on 2024-02-04 19:24_

---

_Branch deleted on 2024-02-04 19:24_

---

_Label `internal` removed by @charliermarsh on 2024-02-04 19:25_

---

_Label `performance` added by @charliermarsh on 2024-02-04 19:25_

---

_Comment by @github-actions[bot] on 2024-02-04 19:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -3 violations, +0 -0 fixes in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/providers/databricks/operators/databricks_repos.py#L105'>airflow/providers/databricks/operators/databricks_repos.py:105:9:</a> ANN205 Missing return type annotation for staticmethod `__detect_repo_provider__`
- <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/providers/databricks/operators/databricks_repos.py#L105'>airflow/providers/databricks/operators/databricks_repos.py:105:9:</a> ANN205 Missing return type annotation for staticmethod `__detect_repo_provider__`
+ <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/providers/fab/auth_manager/models/__init__.py#L81'>airflow/providers/fab/auth_manager/models/__init__.py:81:9:</a> ANN204 Missing return type annotation for special method `__neq__`
- <a href='https://github.com/apache/airflow/blob/46470aba68e5ebeee24a03dc22d012a50ee287ad/airflow/providers/fab/auth_manager/models/__init__.py#L81'>airflow/providers/fab/auth_manager/models/__init__.py:81:9:</a> ANN204 Missing return type annotation for special method `__neq__`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/test_serialization.py#L806'>tests/unit/bokeh/core/test_serialization.py:806:17:</a> ANN204 Missing return type annotation for special method `__array__`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/test_serialization.py#L806'>tests/unit/bokeh/core/test_serialization.py:806:17:</a> ANN204 Missing return type annotation for special method `__array__`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ANN204 | 4 | 2 | 2 | 0 | 0 |
| ANN205 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---
