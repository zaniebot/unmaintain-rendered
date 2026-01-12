```yaml
number: 6700
title: "Add docs for `E275`, `E231`, `E251`, and `E252`"
type: pull_request
state: merged
author: harupy
labels:
  - documentation
assignees: []
merged: true
base: main
head: E275-doc
created_at: 2023-08-20T07:31:17Z
updated_at: 2023-08-20T15:14:13Z
url: https://github.com/astral-sh/ruff/pull/6700
synced_at: 2026-01-12T15:55:22Z
```

# Add docs for `E275`, `E231`, `E251`, and `E252`

---

_@harupy_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

#2646

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-08-20 07:44_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.8±0.06ms    10.7 MB/sec    1.00      3.8±0.05ms    10.7 MB/sec
formatter/numpy/ctypeslib.py               1.00    806.2±9.71µs    20.7 MB/sec    1.00   802.3±11.17µs    20.8 MB/sec
formatter/numpy/globals.py                 1.00     84.2±1.15µs    35.1 MB/sec    1.00     83.9±1.53µs    35.2 MB/sec
formatter/pydantic/types.py                1.00  1553.5±21.15µs    16.4 MB/sec    1.00  1555.8±17.53µs    16.4 MB/sec
linter/all-rules/large/dataset.py          1.05     12.6±0.10ms     3.2 MB/sec    1.00     12.0±0.10ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.3±0.03ms     5.0 MB/sec    1.00      3.2±0.08ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    463.9±3.33µs     6.4 MB/sec    1.00    459.3±5.93µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.04      6.5±0.06ms     3.9 MB/sec    1.00      6.2±0.06ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.09      7.0±0.07ms     5.8 MB/sec    1.00      6.4±0.04ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1489.9±19.53µs    11.2 MB/sec    1.00  1430.1±10.93µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    170.6±2.18µs    17.3 MB/sec    1.00    168.1±1.55µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.06      3.1±0.03ms     8.2 MB/sec    1.00      2.9±0.02ms     8.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.8±0.06ms    10.8 MB/sec    1.00      3.8±0.07ms    10.7 MB/sec
formatter/numpy/ctypeslib.py               1.00   771.8±20.12µs    21.6 MB/sec    1.00   773.1±17.19µs    21.5 MB/sec
formatter/numpy/globals.py                 1.00     79.0±1.39µs    37.4 MB/sec    1.01     80.0±1.98µs    36.9 MB/sec
formatter/pydantic/types.py                1.00  1553.7±28.85µs    16.4 MB/sec    1.01  1562.6±34.74µs    16.3 MB/sec
linter/all-rules/large/dataset.py          1.00     13.3±0.17ms     3.1 MB/sec    1.01     13.4±0.20ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.05ms     4.7 MB/sec    1.00      3.6±0.04ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    443.1±8.81µs     6.7 MB/sec    1.00    443.2±8.32µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.16ms     3.6 MB/sec    1.00      7.0±0.12ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.10ms     5.5 MB/sec    1.00      7.4±0.07ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1534.5±20.64µs    10.9 MB/sec    1.00  1539.8±34.87µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    175.4±3.32µs    16.8 MB/sec    1.01    177.6±4.53µs    16.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.04ms     7.8 MB/sec    1.00      3.3±0.05ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Add doc for `E275`" to "Add docs for `E275`, `E231`" by @harupy on 2023-08-20 12:29_

---

_Renamed from "Add docs for `E275`, `E231`" to "Add docs for `E275`, `E231`, `E251`, and `E252`" by @harupy on 2023-08-20 12:39_

---

_Label `documentation` added by @charliermarsh on 2023-08-20 14:43_

---

_Merged by @charliermarsh on 2023-08-20 14:51_

---

_Closed by @charliermarsh on 2023-08-20 14:51_

---
