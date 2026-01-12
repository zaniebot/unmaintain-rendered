```yaml
number: 7396
title: "Mark `PERF403` as a preview rule"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/perf403
created_at: 2023-09-15T01:48:04Z
updated_at: 2023-09-15T02:04:00Z
url: https://github.com/astral-sh/ruff/pull/7396
synced_at: 2026-01-12T02:39:10Z
```

# Mark `PERF403` as a preview rule

---

_Pull request opened by @charliermarsh on 2023-09-15 01:48_

_No description provided._

---

_Label `rule` added by @charliermarsh on 2023-09-15 01:48_

---

_Merged by @charliermarsh on 2023-09-15 01:57_

---

_Closed by @charliermarsh on 2023-09-15 01:57_

---

_Branch deleted on 2023-09-15 01:57_

---

_Comment by @github-actions[bot] on 2023-09-15 02:04_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -22, 0 error(s))

<details><summary>airflow (+0, -5)</summary>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/8ecd576de1043dbea40e5e16b5dc34859cc41725/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_utils.py#L163'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor_utils.py:163:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
- <a href='https://github.com/apache/airflow/blob/8ecd576de1043dbea40e5e16b5dc34859cc41725/airflow/providers/exasol/hooks/exasol.py#L68'>airflow/providers/exasol/hooks/exasol.py:68:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
- <a href='https://github.com/apache/airflow/blob/8ecd576de1043dbea40e5e16b5dc34859cc41725/airflow/providers/mysql/hooks/mysql.py#L170'>airflow/providers/mysql/hooks/mysql.py:170:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
- <a href='https://github.com/apache/airflow/blob/8ecd576de1043dbea40e5e16b5dc34859cc41725/airflow/providers/postgres/hooks/postgres.py#L153'>airflow/providers/postgres/hooks/postgres.py:153:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
- <a href='https://github.com/apache/airflow/blob/8ecd576de1043dbea40e5e16b5dc34859cc41725/tests/providers/apache/spark/hooks/test_spark_submit.py#L72'>tests/providers/apache/spark/hooks/test_spark_submit.py:72:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
</pre>

</p>
</details>
<details><summary>rotki (+0, -3)</summary>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/4aa415223dd9530d3e1aff76861a17add07aee13/rotkehlchen/api/rest.py#L506'>rotkehlchen/api/rest.py:506:13:</a> PERF403 Use a dictionary comprehension instead of a for-loop
- <a href='https://github.com/rotki/rotki/blob/4aa415223dd9530d3e1aff76861a17add07aee13/rotkehlchen/globaldb/handler.py#L899'>rotkehlchen/globaldb/handler.py:899:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
- <a href='https://github.com/rotki/rotki/blob/4aa415223dd9530d3e1aff76861a17add07aee13/rotkehlchen/tests/utils/database.py#L104'>rotkehlchen/tests/utils/database.py:104:13:</a> PERF403 Use a dictionary comprehension instead of a for-loop
</pre>

</p>
</details>
<details><summary>sphinx (+0, -13)</summary>
<p>

<pre>
- 
-    |
-    |             ^^^^^^^^^^^^ PERF403
- 28 |     for info in reversed(list(request.node.iter_markers("apidoc"))):
- 29 |         for i, a in enumerate(info.args):
- 30 |             pargs[i] = a
- 31 |         kwargs.update(info.kwargs)
- 76 |     for info in reversed(list(request.node.iter_markers("sphinx"))):
- 77 |         for i, a in enumerate(info.args):
- 78 |             pargs[i] = a
- 79 |         kwargs.update(info.kwargs)
- <a href='https://github.com/sphinx-doc/sphinx/blob/f0c25a0adafe36eb987bae6ea13ed4a57d1c3bf0/sphinx/testing/fixtures.py#L78'>sphinx/testing/fixtures.py:78:13:</a> PERF403 Use a dictionary comprehension instead of a for-loop
- <a href='https://github.com/sphinx-doc/sphinx/blob/f0c25a0adafe36eb987bae6ea13ed4a57d1c3bf0/tests/test_ext_apidoc.py#L30'>tests/test_ext_apidoc.py:30:13:</a> PERF403 Use a dictionary comprehension instead of a for-loop
</pre>

</p>
</details>
<details><summary>zulip (+0, -1)</summary>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/db8229ae32475ec9d114d04a35a50c5096d27f2e/analytics/views/stats.py#L456'>analytics/views/stats.py:456:9:</a> PERF403 Use a dictionary comprehension instead of a for-loop
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PERF403 | 11 | 0 | 11 |



---
