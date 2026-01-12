```yaml
number: 6015
title: "[`pylint`] Impement `self-assigning-variable` (`W0127`)"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
assignees: []
merged: true
base: main
head: self-assigning-variable
created_at: 2023-07-23T12:05:38Z
updated_at: 2023-07-24T22:30:41Z
url: https://github.com/astral-sh/ruff/pull/6015
synced_at: 2026-01-12T03:30:22Z
```

# [`pylint`] Impement `self-assigning-variable` (`W0127`)

---

_Pull request opened by @tjkuson on 2023-07-23 12:05_

## Summary

Implements Pylint rule [`self-assigning-variable` (`W0127`)](https://pylint.pycqa.org/en/latest/user_guide/messages/warning/self-assigning-variable.html) as `self-assigning-variable` (`PLW0127`). Includes documentation. Related to #970.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-07-23 12:16_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+1, -0, 0 error(s))

<details><summary>airflow (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/73bc49adb17957e5bb8dee357c04534c6b41f9dd/airflow/settings.py#L91'>airflow/settings.py:91:1:</a> PLW0127 Self-assignment of variable `json`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PLW0127 | 1 | 1 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.01ms     4.4 MB/sec    1.00      9.3±0.03ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1871.7±3.61µs     8.9 MB/sec    1.00   1869.3±7.30µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    210.0±1.11µs    14.1 MB/sec    1.02   213.7±17.47µs    13.8 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.3 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     12.9±0.02ms     3.2 MB/sec    1.01     13.0±0.02ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.00ms     5.1 MB/sec    1.00      3.3±0.00ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    432.6±0.77µs     6.8 MB/sec    1.00    431.6±0.75µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.02ms     4.4 MB/sec    1.00      5.9±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.01ms     6.2 MB/sec    1.01      6.6±0.01ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1405.8±1.26µs    11.8 MB/sec    1.00   1408.4±1.23µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    156.7±0.61µs    18.8 MB/sec    1.00    156.7±0.33µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.00ms     8.6 MB/sec    1.01      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04     11.4±0.18ms     3.6 MB/sec    1.00     11.0±0.02ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.2±0.01ms     7.7 MB/sec    1.00      2.1±0.01ms     7.9 MB/sec
formatter/numpy/globals.py                 1.03    238.3±2.09µs    12.4 MB/sec    1.00    230.4±3.26µs    12.8 MB/sec
formatter/pydantic/types.py                1.03      4.8±0.02ms     5.3 MB/sec    1.00      4.7±0.03ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.1±0.05ms     2.7 MB/sec    1.00     15.2±0.03ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.02ms     4.1 MB/sec    1.00      4.0±0.02ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    410.5±5.46µs     7.2 MB/sec    1.00    409.4±4.76µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.02ms     3.7 MB/sec    1.01      7.0±0.03ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.02ms     5.1 MB/sec    1.01      8.0±0.03ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1615.7±10.95µs    10.3 MB/sec    1.00   1615.3±6.23µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    172.5±1.32µs    17.1 MB/sec    1.00    171.5±1.27µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.02ms     7.2 MB/sec    1.01      3.6±0.01ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/mod.rs`:1661 on 2023-07-23 15:40_

Value is already extracted two rules above here. Perhaps group those

---

_@sbrugman reviewed on 2023-07-23 15:41_

---

_Label `rule` added by @charliermarsh on 2023-07-24 02:13_

---

_Merged by @charliermarsh on 2023-07-24 02:27_

---

_Closed by @charliermarsh on 2023-07-24 02:27_

---

_Branch deleted on 2023-07-24 22:30_

---
