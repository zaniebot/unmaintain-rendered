```yaml
number: 11250
title: "Ignore list-copy recommendations for async `for` loops"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/async
created_at: 2024-05-02T18:37:33Z
updated_at: 2024-05-02T18:49:53Z
url: https://github.com/astral-sh/ruff/pull/11250
synced_at: 2026-01-12T15:55:37Z
```

# Ignore list-copy recommendations for async `for` loops

---

_@charliermarsh_

## Summary

Removes these from `PERF402`, but adds them to `PERF401`, with a custom message to use an `async` comprehension.

Closes https://github.com/astral-sh/ruff/issues/10787.


---

_@zanieb approved on 2024-05-02 18:39_

---

_Comment by @zanieb on 2024-05-02 18:39_

Nice!

---

_Merged by @charliermarsh on 2024-05-02 18:48_

---

_Closed by @charliermarsh on 2024-05-02 18:48_

---

_Branch deleted on 2024-05-02 18:48_

---

_Comment by @github-actions[bot] on 2024-05-02 18:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -2 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/598398a81657c06e092d1290200a9facf82f55f3/airflow/providers/microsoft/azure/hooks/wasb.py#L690'>airflow/providers/microsoft/azure/hooks/wasb.py:690:13:</a> PERF401 Use an async list comprehension to create a transformed list
- <a href='https://github.com/apache/airflow/blob/598398a81657c06e092d1290200a9facf82f55f3/airflow/providers/microsoft/azure/hooks/wasb.py#L690'>airflow/providers/microsoft/azure/hooks/wasb.py:690:13:</a> PERF402 Use `list` or `list.copy` to create a copy of a list
+ <a href='https://github.com/apache/airflow/blob/598398a81657c06e092d1290200a9facf82f55f3/tests/providers/microsoft/azure/base.py#L55'>tests/providers/microsoft/azure/base.py:55:13:</a> PERF401 Use an async list comprehension to create a transformed list
- <a href='https://github.com/apache/airflow/blob/598398a81657c06e092d1290200a9facf82f55f3/tests/providers/microsoft/azure/base.py#L55'>tests/providers/microsoft/azure/base.py:55:13:</a> PERF402 Use `list` or `list.copy` to create a copy of a list
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PERF401 | 2 | 2 | 0 | 0 | 0 |
| PERF402 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -2 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/598398a81657c06e092d1290200a9facf82f55f3/airflow/providers/microsoft/azure/hooks/wasb.py#L690'>airflow/providers/microsoft/azure/hooks/wasb.py:690:13:</a> PERF401 Use an async list comprehension to create a transformed list
- <a href='https://github.com/apache/airflow/blob/598398a81657c06e092d1290200a9facf82f55f3/airflow/providers/microsoft/azure/hooks/wasb.py#L690'>airflow/providers/microsoft/azure/hooks/wasb.py:690:13:</a> PERF402 Use `list` or `list.copy` to create a copy of a list
+ <a href='https://github.com/apache/airflow/blob/598398a81657c06e092d1290200a9facf82f55f3/tests/providers/microsoft/azure/base.py#L55'>tests/providers/microsoft/azure/base.py:55:13:</a> PERF401 Use an async list comprehension to create a transformed list
- <a href='https://github.com/apache/airflow/blob/598398a81657c06e092d1290200a9facf82f55f3/tests/providers/microsoft/azure/base.py#L55'>tests/providers/microsoft/azure/base.py:55:13:</a> PERF402 Use `list` or `list.copy` to create a copy of a list
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PERF401 | 2 | 2 | 0 | 0 | 0 |
| PERF402 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>




---
