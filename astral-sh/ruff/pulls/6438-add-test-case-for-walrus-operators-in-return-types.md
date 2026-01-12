```yaml
number: 6438
title: Add test case for walrus operators in return types
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/walrus
created_at: 2023-08-09T04:44:07Z
updated_at: 2023-08-11T18:48:32Z
url: https://github.com/astral-sh/ruff/pull/6438
synced_at: 2026-01-12T02:52:04Z
```

# Add test case for walrus operators in return types

---

_Pull request opened by @charliermarsh on 2023-08-09 04:44_

## Summary

Closes https://github.com/astral-sh/ruff/issues/6437.

## Test Plan

`cargo test`


---

_Label `formatter` added by @charliermarsh on 2023-08-09 04:46_

---

_@charliermarsh reviewed on 2023-08-09 04:47_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/expr_named_expr.rs`:52 on 2023-08-09 04:47_

For completeness: this change alone was actually not sufficient; we _also_ need https://github.com/astral-sh/ruff/pull/6436, which ensures that we respect "required" parentheses for nodes.

---

_Comment by @github-actions[bot] on 2023-08-09 05:29_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.04ms     4.9 MB/sec    1.00      8.3±0.07ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1652.5±31.47µs    10.1 MB/sec    1.00  1654.1±37.57µs    10.1 MB/sec
formatter/numpy/globals.py                 1.00    190.4±7.25µs    15.5 MB/sec    1.02    193.3±7.66µs    15.3 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.06ms     7.3 MB/sec    1.00      3.5±0.11ms     7.3 MB/sec
linter/all-rules/large/dataset.py          1.00     10.3±0.06ms     3.9 MB/sec    1.02     10.6±0.04ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.03ms     6.0 MB/sec    1.01      2.8±0.01ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    392.4±0.98µs     7.5 MB/sec    1.00    393.0±0.96µs     7.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.02ms     4.8 MB/sec    1.02      5.5±0.04ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.02ms     7.5 MB/sec    1.06      5.8±0.03ms     7.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1219.7±23.48µs    13.7 MB/sec    1.02   1248.3±7.21µs    13.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    141.8±0.55µs    20.8 MB/sec    1.03    145.8±3.79µs    20.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.02ms    10.3 MB/sec    1.05      2.6±0.04ms     9.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.05     13.9±0.57ms     2.9 MB/sec    1.00     13.2±0.66ms     3.1 MB/sec
formatter/numpy/ctypeslib.py               1.09      2.7±0.22ms     6.2 MB/sec    1.00      2.5±0.15ms     6.8 MB/sec
formatter/numpy/globals.py                 1.00   282.0±19.68µs    10.5 MB/sec    1.02   287.6±19.56µs    10.3 MB/sec
formatter/pydantic/types.py                1.08      5.9±0.26ms     4.3 MB/sec    1.00      5.5±0.24ms     4.7 MB/sec
linter/all-rules/large/dataset.py          1.00     16.6±0.57ms     2.4 MB/sec    1.04     17.2±1.24ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.6±0.37ms     3.6 MB/sec    1.00      4.6±0.30ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   571.6±42.55µs     5.2 MB/sec    1.01   576.6±28.26µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.7±0.36ms     2.9 MB/sec    1.00      8.7±0.41ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.01      9.2±0.36ms     4.4 MB/sec    1.00      9.2±0.36ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1978.5±96.25µs     8.4 MB/sec    1.00  1983.5±103.11µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   228.9±15.58µs    12.9 MB/sec    1.02   232.5±13.77µs    12.7 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.2±0.20ms     6.1 MB/sec    1.00      4.1±0.19ms     6.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-08-09 06:16_

---

_@MichaReiser approved on 2023-08-09 06:40_

---

_Merged by @charliermarsh on 2023-08-11 18:28_

---

_Closed by @charliermarsh on 2023-08-11 18:28_

---

_Branch deleted on 2023-08-11 18:28_

---
