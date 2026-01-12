```yaml
number: 6490
title: "Allow `os._exit` accesses in `SLF001`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/slf
created_at: 2023-08-11T00:43:00Z
updated_at: 2023-08-11T01:18:30Z
url: https://github.com/astral-sh/ruff/pull/6490
synced_at: 2026-01-12T02:52:04Z
```

# Allow `os._exit` accesses in `SLF001`

---

_Pull request opened by @charliermarsh on 2023-08-11 00:43_

Closes https://github.com/astral-sh/ruff/issues/6483.

---

_Marked ready for review by @charliermarsh on 2023-08-11 00:43_

---

_Label `bug` added by @charliermarsh on 2023-08-11 00:43_

---

_Renamed from "Allow `os._exit` accesses" to "Allow `os._exit` accesses in `SLF001`" by @charliermarsh on 2023-08-11 00:43_

---

_Merged by @charliermarsh on 2023-08-11 00:54_

---

_Closed by @charliermarsh on 2023-08-11 00:54_

---

_Branch deleted on 2023-08-11 00:54_

---

_Comment by @github-actions[bot] on 2023-08-11 00:58_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -5, 0 error(s))

<details><summary>airflow (+0, -5)</summary>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/dcfca57eb53840d396ceec25e73c09204bd65d35/airflow/executors/local_executor.py#L140'>airflow/executors/local_executor.py:140:13:</a> SLF001 Private member accessed: `_exit`
- <a href='https://github.com/apache/airflow/blob/dcfca57eb53840d396ceec25e73c09204bd65d35/airflow/models/taskinstance.py#L1630'>airflow/models/taskinstance.py:1630:17:</a> SLF001 Private member accessed: `_exit`
- <a href='https://github.com/apache/airflow/blob/dcfca57eb53840d396ceec25e73c09204bd65d35/airflow/providers/celery/executors/celery_executor_utils.py#L164'>airflow/providers/celery/executors/celery_executor_utils.py:164:9:</a> SLF001 Private member accessed: `_exit`
- <a href='https://github.com/apache/airflow/blob/dcfca57eb53840d396ceec25e73c09204bd65d35/airflow/task/task_runner/standard_task_runner.py#L137'>airflow/task/task_runner/standard_task_runner.py:137:13:</a> SLF001 Private member accessed: `_exit`
- <a href='https://github.com/apache/airflow/blob/dcfca57eb53840d396ceec25e73c09204bd65d35/dev/provider_packages/prepare_provider_packages.py#L2092'>dev/provider_packages/prepare_provider_packages.py:2092:13:</a> SLF001 Private member accessed: `_exit`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| SLF001 | 5 | 0 | 5 |

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      8.3±0.39ms     4.9 MB/sec     1.11      9.1±0.57ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1575.5±102.32µs    10.6 MB/sec    1.15  1811.0±97.02µs     9.2 MB/sec
formatter/numpy/globals.py                 1.00   188.7±15.45µs    15.6 MB/sec     1.17   220.4±19.23µs    13.4 MB/sec
formatter/pydantic/types.py                1.00      3.4±0.26ms     7.4 MB/sec     1.21      4.1±0.22ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.00     11.0±0.78ms     3.7 MB/sec     1.00     11.0±0.79ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.0±0.20ms     5.5 MB/sec     1.00      3.0±0.15ms     5.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   413.1±23.97µs     7.1 MB/sec     1.13   465.9±32.26µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.6±0.31ms     4.5 MB/sec     1.05      5.9±0.20ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.0±0.45ms     6.8 MB/sec     1.04      6.2±0.55ms     6.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1326.0±90.74µs    12.6 MB/sec     1.00  1312.1±77.83µs    12.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    150.2±9.86µs    19.6 MB/sec     1.17   175.8±10.05µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.25ms     9.1 MB/sec     1.01      2.8±0.19ms     9.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.0±0.05ms     4.1 MB/sec    1.02     10.2±0.04ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1913.5±14.30µs     8.7 MB/sec    1.01   1932.7±9.01µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    198.9±2.03µs    14.8 MB/sec    1.02   203.6±10.83µs    14.5 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.12ms     5.9 MB/sec    1.00      4.3±0.04ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     12.7±0.08ms     3.2 MB/sec    1.01     12.8±0.18ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.03ms     4.7 MB/sec    1.00      3.5±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    370.4±5.88µs     8.0 MB/sec    1.00    365.7±4.87µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.6±0.05ms     3.8 MB/sec    1.00      6.6±0.05ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.01      6.9±0.05ms     5.9 MB/sec    1.00      6.8±0.05ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1430.6±13.19µs    11.6 MB/sec    1.00   1404.0±9.90µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    143.8±1.44µs    20.5 MB/sec    1.00    143.1±1.45µs    20.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.02ms     8.2 MB/sec    1.00      3.1±0.02ms     8.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
