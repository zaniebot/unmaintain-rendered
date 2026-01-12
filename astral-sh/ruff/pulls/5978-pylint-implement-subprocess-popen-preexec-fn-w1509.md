```yaml
number: 5978
title: "[`pylint`] Implement `subprocess-popen-preexec-fn`  (`W1509`)"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
assignees: []
merged: true
base: main
head: preexec-fn
created_at: 2023-07-22T13:25:54Z
updated_at: 2023-07-24T22:30:40Z
url: https://github.com/astral-sh/ruff/pull/5978
synced_at: 2026-01-12T15:55:20Z
```

# [`pylint`] Implement `subprocess-popen-preexec-fn`  (`W1509`)

---

_@tjkuson_

## Summary

Implements Pylint rule [`subprocess-popen-preexec-fn` (`W1509`)](https://pylint.pycqa.org/en/latest/user_guide/messages/warning/subprocess-popen-preexec-fn.html) as `subprocess-popen-preexec-fn` (`PLW1509`). Includes documentation. Related to #970.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-07-22 13:40_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+3, -0, 0 error(s))

<details><summary>airflow (+3, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/73bc49adb17957e5bb8dee357c04534c6b41f9dd/airflow/hooks/subprocess.py#L83'>airflow/hooks/subprocess.py:83:17:</a> PLW1509 `preexec_fn` argument is unsafe when using threads
+ <a href='https://github.com/apache/airflow/blob/73bc49adb17957e5bb8dee357c04534c6b41f9dd/airflow/sensors/bash.py#L87'>airflow/sensors/bash.py:87:21:</a> PLW1509 `preexec_fn` argument is unsafe when using threads
+ <a href='https://github.com/apache/airflow/blob/73bc49adb17957e5bb8dee357c04534c6b41f9dd/airflow/task/task_runner/base_task_runner.py#L151'>airflow/task/task_runner/base_task_runner.py:151:21:</a> PLW1509 `preexec_fn` argument is unsafe when using threads
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PLW1509 | 3 | 3 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.02ms     4.4 MB/sec    1.00      9.3±0.02ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1874.5±2.80µs     8.9 MB/sec    1.00   1865.3±2.53µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    211.4±0.50µs    14.0 MB/sec    1.00    211.4±2.23µs    14.0 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.4 MB/sec    1.01      4.1±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     12.9±0.02ms     3.2 MB/sec    1.00     12.9±0.01ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.1 MB/sec    1.00      3.3±0.00ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    428.2±0.66µs     6.9 MB/sec    1.00    427.3±0.77µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.02ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.6±0.02ms     6.2 MB/sec    1.00      6.5±0.01ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1407.2±1.08µs    11.8 MB/sec    1.00   1398.2±1.06µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    155.1±0.18µs    19.0 MB/sec    1.00    155.0±1.13µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.6 MB/sec    1.00      3.0±0.01ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.1±0.07ms     3.7 MB/sec    1.01     11.2±0.19ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.02ms     7.7 MB/sec    1.01      2.2±0.03ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    232.2±7.10µs    12.7 MB/sec    1.00    233.2±6.10µs    12.7 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.03ms     5.4 MB/sec    1.01      4.8±0.05ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.01     15.8±0.14ms     2.6 MB/sec    1.00     15.7±0.11ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.04ms     4.0 MB/sec    1.00      4.1±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    418.0±5.52µs     7.1 MB/sec    1.00    416.6±5.63µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.07ms     3.6 MB/sec    1.00      7.1±0.06ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.09ms     5.0 MB/sec    1.00      8.2±0.08ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1647.1±11.69µs    10.1 MB/sec    1.00  1649.2±12.91µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    175.3±1.44µs    16.8 MB/sec    1.00    175.5±1.78µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     7.0 MB/sec    1.00      3.7±0.03ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "[`pylint`] Impliment `subprocess-popen-preexec-fn`  (`W1509`)" to "[`pylint`] Implement `subprocess-popen-preexec-fn`  (`W1509`)" by @tjkuson on 2023-07-22 14:03_

---

_Review comment by @sbrugman on `crates/ruff/src/rules/pylint/rules/subprocess_popen_preexec_fn.rs`:60 on 2023-07-22 14:07_

You can use the `find_keyword` helper function here

---

_@sbrugman reviewed on 2023-07-22 14:07_

---

_@tjkuson reviewed on 2023-07-22 14:12_

---

_Review comment by @tjkuson on `crates/ruff/src/rules/pylint/rules/subprocess_popen_preexec_fn.rs`:60 on 2023-07-22 14:12_

Good call.

---

_Label `rule` added by @charliermarsh on 2023-07-24 01:59_

---

_Merged by @charliermarsh on 2023-07-24 02:06_

---

_Closed by @charliermarsh on 2023-07-24 02:06_

---

_Branch deleted on 2023-07-24 22:30_

---
