```yaml
number: 5389
title: Add applicability to flake8_pytest_style
type: pull_request
state: merged
author: evanrittenhouse
labels: []
assignees: []
merged: true
base: main
head: applicability_flake8_pytest_style
created_at: 2023-06-27T13:41:03Z
updated_at: 2023-06-27T16:39:58Z
url: https://github.com/astral-sh/ruff/pull/5389
synced_at: 2026-01-12T03:36:55Z
```

# Add applicability to flake8_pytest_style

---

_Pull request opened by @evanrittenhouse on 2023-06-27 13:41_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes part of #4184 

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-06-27 13:50_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      7.9±0.07ms     5.1 MB/sec    1.00      7.7±0.04ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.01  1678.3±11.21µs     9.9 MB/sec    1.00   1663.0±5.32µs    10.0 MB/sec
formatter/numpy/globals.py                 1.00    186.7±0.36µs    15.8 MB/sec    1.00    186.9±0.24µs    15.8 MB/sec
formatter/pydantic/types.py                1.01      3.7±0.01ms     6.9 MB/sec    1.00      3.6±0.01ms     7.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.2±0.19ms     3.1 MB/sec    1.00     13.3±0.17ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.1 MB/sec    1.00      3.3±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    424.7±1.09µs     6.9 MB/sec    1.03    436.3±2.55µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.8±0.04ms     4.4 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.6±0.04ms     6.1 MB/sec    1.00      6.6±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1452.5±3.84µs    11.5 MB/sec    1.00   1446.1±4.43µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.9±0.90µs    18.1 MB/sec    1.00    163.5±0.30µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.03ms     8.4 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.2±0.04ms     4.4 MB/sec    1.00      9.2±0.03ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00  1901.3±11.35µs     8.8 MB/sec    1.00   1901.4±9.86µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    208.4±1.22µs    14.2 MB/sec    1.02   213.3±10.86µs    13.8 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.02ms     5.9 MB/sec    1.00      4.3±0.02ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     15.1±0.11ms     2.7 MB/sec    1.00     15.2±0.08ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.03ms     4.1 MB/sec    1.00      4.1±0.02ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    422.8±4.29µs     7.0 MB/sec    1.00    423.6±5.38µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.02ms     3.7 MB/sec    1.00      6.8±0.02ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.15ms     5.1 MB/sec    1.00      8.0±0.02ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1661.0±39.21µs    10.0 MB/sec    1.00   1650.5±8.51µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.0±1.10µs    16.5 MB/sec    1.00    178.8±1.48µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.1 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-27 16:39_

---

_Closed by @charliermarsh on 2023-06-27 16:39_

---
