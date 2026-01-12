```yaml
number: 6264
title: "Add `PT025` and `PT026` docs"
type: pull_request
state: merged
author: harupy
labels:
  - documentation
assignees: []
merged: true
base: main
head: PT025-PT026-docs
created_at: 2023-08-02T06:54:04Z
updated_at: 2023-08-02T19:31:12Z
url: https://github.com/astral-sh/ruff/pull/6264
synced_at: 2026-01-12T02:58:30Z
```

# Add `PT025` and `PT026` docs

---

_Pull request opened by @harupy on 2023-08-02 06:54_

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

_Comment by @github-actions[bot] on 2023-08-02 07:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.5±0.07ms     4.8 MB/sec    1.00      8.4±0.06ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1651.6±34.46µs    10.1 MB/sec    1.01  1662.0±28.90µs    10.0 MB/sec
formatter/numpy/globals.py                 1.00    172.1±0.75µs    17.1 MB/sec    1.00    172.4±0.19µs    17.1 MB/sec
formatter/pydantic/types.py                1.01      3.6±0.07ms     7.0 MB/sec    1.00      3.6±0.04ms     7.1 MB/sec
linter/all-rules/large/dataset.py          1.00     11.3±0.08ms     3.6 MB/sec    1.01     11.4±0.21ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.02ms     5.7 MB/sec    1.00      2.9±0.01ms     5.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    315.4±2.21µs     9.4 MB/sec    1.02    322.3±1.76µs     9.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.1±0.03ms     5.0 MB/sec    1.00      5.1±0.01ms     5.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.0±0.03ms     6.8 MB/sec    1.01      6.0±0.02ms     6.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1196.5±16.48µs    13.9 MB/sec    1.01  1206.0±10.27µs    13.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    120.8±0.44µs    24.4 MB/sec    1.00    119.9±0.83µs    24.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.6±0.01ms     9.7 MB/sec    1.01      2.6±0.01ms     9.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.10ms     4.1 MB/sec    1.08     10.7±0.12ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1939.1±33.13µs     8.6 MB/sec    1.06      2.0±0.05ms     8.1 MB/sec
formatter/numpy/globals.py                 1.00    214.7±4.17µs    13.7 MB/sec    1.04    223.1±8.17µs    13.2 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.08ms     6.0 MB/sec    1.07      4.5±0.07ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.00     13.3±0.10ms     3.1 MB/sec    1.01     13.5±0.13ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.04ms     4.7 MB/sec    1.01      3.6±0.04ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    433.5±8.03µs     6.8 MB/sec    1.01    437.7±7.95µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.08ms     4.2 MB/sec    1.01      6.1±0.07ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.05ms     5.6 MB/sec    1.00      7.3±0.07ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1495.9±16.46µs    11.1 MB/sec    1.00  1501.8±35.35µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    169.1±2.70µs    17.5 MB/sec    1.00    167.9±2.79µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.04ms     7.9 MB/sec    1.00      3.2±0.05ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-08-02 18:50_

---

_Merged by @charliermarsh on 2023-08-02 19:00_

---

_Closed by @charliermarsh on 2023-08-02 19:00_

---

_Branch deleted on 2023-08-02 19:31_

---
