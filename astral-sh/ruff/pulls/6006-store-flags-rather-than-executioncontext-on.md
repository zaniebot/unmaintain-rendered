```yaml
number: 6006
title: "Store flags rather than `ExecutionContext` on references"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/flags
created_at: 2023-07-23T02:43:41Z
updated_at: 2023-07-23T03:14:58Z
url: https://github.com/astral-sh/ruff/pull/6006
synced_at: 2026-01-12T03:30:22Z
```

# Store flags rather than `ExecutionContext` on references

---

_Pull request opened by @charliermarsh on 2023-07-23 02:43_

_No description provided._

---

_Marked ready for review by @charliermarsh on 2023-07-23 02:43_

---

_Renamed from "Store flags rather than ExecutionContext on references" to "Store flags rather than `ExecutionContext` on references" by @charliermarsh on 2023-07-23 02:43_

---

_Merged by @charliermarsh on 2023-07-23 02:54_

---

_Closed by @charliermarsh on 2023-07-23 02:54_

---

_Branch deleted on 2023-07-23 02:54_

---

_Comment by @github-actions[bot] on 2023-07-23 02:58_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+5, -0, 0 error(s))

<details><summary>airflow (+5, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/56c41d460c3f2a4e871c7834033c3152e71f71d2/airflow/__init__.py#L129'>airflow/__init__.py:129:36:</a> TCH001 [*] Move application import `airflow.models.dag.DAG` into a type-checking block
+ <a href='https://github.com/apache/airflow/blob/56c41d460c3f2a4e871c7834033c3152e71f71d2/airflow/__init__.py#L130'>airflow/__init__.py:130:41:</a> TCH001 [*] Move application import `airflow.models.xcom_arg.XComArg` into a type-checking block
+ <a href='https://github.com/apache/airflow/blob/56c41d460c3f2a4e871c7834033c3152e71f71d2/airflow/decorators/__init__.py#L21'>airflow/decorators/__init__.py:21:37:</a> TCH001 [*] Move application import `airflow.decorators.base.TaskDecorator` into a type-checking block
+ <a href='https://github.com/apache/airflow/blob/56c41d460c3f2a4e871c7834033c3152e71f71d2/airflow/decorators/__init__.py#L29'>airflow/decorators/__init__.py:29:43:</a> TCH001 [*] Move application import `airflow.decorators.task_group.task_group` into a type-checking block
+ <a href='https://github.com/apache/airflow/blob/56c41d460c3f2a4e871c7834033c3152e71f71d2/airflow/decorators/__init__.py#L30'>airflow/decorators/__init__.py:30:32:</a> TCH001 [*] Move application import `airflow.models.dag.dag` into a type-checking block
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| TCH001 | 5 | 5 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.04ms     4.4 MB/sec    1.01      9.4±0.05ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1853.4±1.66µs     9.0 MB/sec    1.01  1866.0±15.98µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    202.6±0.28µs    14.6 MB/sec    1.01    203.9±3.60µs    14.5 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.3 MB/sec    1.00      4.0±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.01     13.0±0.06ms     3.1 MB/sec    1.00     12.9±0.06ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.1 MB/sec    1.00      3.3±0.00ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    427.9±0.63µs     6.9 MB/sec    1.00    426.2±0.72µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.02ms     4.3 MB/sec    1.00      5.9±0.05ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.02ms     6.2 MB/sec    1.00      6.6±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1398.7±1.46µs    11.9 MB/sec    1.00   1404.4±4.53µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    154.4±0.21µs    19.1 MB/sec    1.00    154.8±0.26µs    19.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.6 MB/sec    1.00      3.0±0.01ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.2±0.15ms     3.6 MB/sec    1.02     11.3±0.17ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.03ms     7.6 MB/sec    1.02      2.2±0.03ms     7.5 MB/sec
formatter/numpy/globals.py                 1.00   244.7±16.02µs    12.1 MB/sec    1.04   255.5±13.31µs    11.5 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.06ms     5.3 MB/sec    1.02      4.9±0.07ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.16ms     2.6 MB/sec    1.00     15.6±0.17ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.07ms     4.1 MB/sec    1.00      4.1±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.02    493.7±9.13µs     6.0 MB/sec    1.00   486.3±13.21µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.1±0.12ms     3.6 MB/sec    1.00      7.1±0.11ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.02      8.2±0.11ms     5.0 MB/sec    1.00      8.1±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1705.1±38.43µs     9.8 MB/sec    1.00  1682.0±31.01µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    191.7±3.58µs    15.4 MB/sec    1.00    190.1±3.00µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.04ms     7.0 MB/sec    1.00      3.6±0.04ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
