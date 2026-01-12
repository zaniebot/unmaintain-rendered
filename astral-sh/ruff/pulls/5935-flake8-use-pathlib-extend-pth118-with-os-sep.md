```yaml
number: 5935
title: "[`flake8-use-pathlib`] extend PTH118 with `os.sep`"
type: pull_request
state: merged
author: sbrugman
labels:
  - rule
assignees: []
merged: true
base: main
head: pth118-os-sep
created_at: 2023-07-21T00:10:47Z
updated_at: 2023-07-21T01:56:40Z
url: https://github.com/astral-sh/ruff/pull/5935
synced_at: 2026-01-12T15:55:20Z
```

# [`flake8-use-pathlib`] extend PTH118 with `os.sep`

---

_@sbrugman_

Closes https://github.com/astral-sh/ruff/issues/5905

---

_Comment by @github-actions[bot] on 2023-07-21 00:21_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+3, -0, 0 error(s))

<details><summary>airflow (+3, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/15d42b4320d535cf54743929f134e36f59c615bb/tests/sensors/test_external_task_sensor.py#L77'>tests/sensors/test_external_task_sensor.py:77:33:</a> PTH118 `os.sep.join()` should be replaced by `Path` with `/` operator
+ <a href='https://github.com/apache/airflow/blob/15d42b4320d535cf54743929f134e36f59c615bb/tests/sensors/test_external_task_sensor.py#L79'>tests/sensors/test_external_task_sensor.py:79:30:</a> PTH118 `os.sep.join()` should be replaced by `Path` with `/` operator
+ <a href='https://github.com/apache/airflow/blob/15d42b4320d535cf54743929f134e36f59c615bb/tests/sensors/test_external_task_sensor.py#L81'>tests/sensors/test_external_task_sensor.py:81:36:</a> PTH118 `os.sep.join()` should be replaced by `Path` with `/` operator
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PTH118 | 3 | 3 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.09ms     4.4 MB/sec    1.00      9.3±0.03ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1851.3±1.63µs     9.0 MB/sec    1.00   1845.2±1.67µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    204.5±0.30µs    14.4 MB/sec    1.00    204.2±0.30µs    14.4 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.4 MB/sec    1.00      4.0±0.05ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     12.9±0.08ms     3.1 MB/sec    1.01     13.1±0.15ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.1 MB/sec    1.00      3.3±0.00ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    435.9±1.23µs     6.8 MB/sec    1.00    433.0±0.60µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.9±0.03ms     4.3 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.02ms     6.2 MB/sec    1.00      6.6±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1413.8±1.57µs    11.8 MB/sec    1.00   1412.8±1.47µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    156.4±0.24µs    18.9 MB/sec    1.00    156.0±0.28µs    18.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.00      3.0±0.01ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.3±0.18ms     3.9 MB/sec    1.00     10.3±0.24ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1979.2±41.78µs     8.4 MB/sec    1.01  1991.3±41.25µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    215.5±6.50µs    13.7 MB/sec    1.03    221.7±9.02µs    13.3 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.09ms     5.8 MB/sec    1.01      4.4±0.10ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     14.5±0.18ms     2.8 MB/sec    1.01     14.6±0.23ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.05ms     4.4 MB/sec    1.00      3.8±0.07ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    448.3±8.52µs     6.6 MB/sec    1.00   450.0±11.54µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.6±0.14ms     3.8 MB/sec    1.00      6.6±0.13ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.09ms     5.4 MB/sec    1.00      7.6±0.09ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1550.3±22.62µs    10.7 MB/sec    1.01  1558.2±28.56µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.6±4.20µs    17.0 MB/sec    1.00    174.1±4.57µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.04ms     7.6 MB/sec    1.00      3.3±0.05ms     7.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @sbrugman on 2023-07-21 00:24_

Ecosystem detected these cases correctly

---

_Label `rule` added by @charliermarsh on 2023-07-21 01:30_

---

_Merged by @charliermarsh on 2023-07-21 01:36_

---

_Closed by @charliermarsh on 2023-07-21 01:36_

---

_Branch deleted on 2023-07-21 01:39_

---
