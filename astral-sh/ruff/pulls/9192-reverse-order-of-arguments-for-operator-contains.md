```yaml
number: 9192
title: "Reverse order of arguments for `operator.contains`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/fix
created_at: 2023-12-18T19:29:56Z
updated_at: 2023-12-18T19:42:25Z
url: https://github.com/astral-sh/ruff/pull/9192
synced_at: 2026-01-10T23:31:11Z
```

# Reverse order of arguments for `operator.contains`

---

_Pull request opened by @charliermarsh on 2023-12-18 19:29_

Closes https://github.com/astral-sh/ruff/issues/9191.

---

_Renamed from "Reverse order of arguments for operator.contains" to "Reverse order of arguments for `operator.contains`" by @charliermarsh on 2023-12-18 19:30_

---

_Label `bug` added by @charliermarsh on 2023-12-18 19:30_

---

_Merged by @charliermarsh on 2023-12-18 19:39_

---

_Closed by @charliermarsh on 2023-12-18 19:39_

---

_Branch deleted on 2023-12-18 19:39_

---

_Comment by @github-actions[bot] on 2023-12-18 19:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -2 violations, +0 -0 fixes in 2 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/329780649543ab7b9d593a2e2428073fbd4cf274/kubernetes_tests/test_base.py#L45'>kubernetes_tests/test_base.py:45:5:</a> FURB118 Use `operator.contains` instead of defining a function
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/core/query.py#L353'>src/bokeh/core/query.py:353:9:</a> FURB118 [*] Use `operator.contains` instead of defining a lambda
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB118 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>




---
