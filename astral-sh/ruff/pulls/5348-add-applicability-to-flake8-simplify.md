```yaml
number: 5348
title: Add Applicability to flake8_simplify
type: pull_request
state: merged
author: evanrittenhouse
labels: []
assignees: []
merged: true
base: main
head: applicability_flake8_simplify
created_at: 2023-06-23T22:38:03Z
updated_at: 2023-06-23T23:17:15Z
url: https://github.com/astral-sh/ruff/pull/5348
synced_at: 2026-01-12T03:36:55Z
```

# Add Applicability to flake8_simplify

---

_Pull request opened by @evanrittenhouse on 2023-06-23 22:38_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Partially fixes #4184.

## Test Plan

<!-- How was it tested? -->


---

_Renamed from "Add applicability to flake8_simplify" to "Add Applicability to flake8_simplify" by @evanrittenhouse on 2023-06-23 22:39_

---

_Merged by @charliermarsh on 2023-06-23 22:54_

---

_Closed by @charliermarsh on 2023-06-23 22:54_

---

_Comment by @github-actions[bot] on 2023-06-23 22:57_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.7±0.04ms     6.0 MB/sec    1.01      6.8±0.04ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   1563.9±1.78µs    10.6 MB/sec    1.01   1584.7±4.02µs    10.5 MB/sec
formatter/numpy/globals.py                 1.00    190.7±1.37µs    15.5 MB/sec    1.00    191.0±0.26µs    15.5 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.02ms     7.3 MB/sec    1.00      3.5±0.01ms     7.3 MB/sec
linter/all-rules/large/dataset.py          1.00     13.4±0.10ms     3.0 MB/sec    1.01     13.5±0.09ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.03ms     5.0 MB/sec    1.03      3.5±0.04ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.03    437.3±2.01µs     6.7 MB/sec    1.00    425.4±0.74µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.0±0.05ms     4.2 MB/sec    1.00      5.9±0.05ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.04ms     6.1 MB/sec    1.02      6.8±0.04ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1447.6±24.82µs    11.5 MB/sec    1.01   1457.0±3.08µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    159.8±0.27µs    18.5 MB/sec    1.01    161.3±0.30µs    18.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.01      3.1±0.01ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.2±0.38ms     4.0 MB/sec    1.00     10.2±0.44ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.10ms     7.2 MB/sec    1.01      2.3±0.11ms     7.1 MB/sec
formatter/numpy/globals.py                 1.00   280.9±16.56µs    10.5 MB/sec    1.03   288.8±21.18µs    10.2 MB/sec
formatter/pydantic/types.py                1.00      5.1±0.23ms     5.0 MB/sec    1.02      5.2±0.25ms     4.9 MB/sec
linter/all-rules/large/dataset.py          1.00     19.6±0.46ms     2.1 MB/sec    1.01     19.8±0.59ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.3±0.23ms     3.2 MB/sec    1.02      5.3±0.24ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   646.5±31.12µs     4.6 MB/sec    1.00   647.1±44.64µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.7±0.32ms     2.9 MB/sec    1.04      9.1±0.38ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.3±0.43ms     3.9 MB/sec    1.01     10.4±0.55ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.11ms     7.4 MB/sec    1.01      2.3±0.17ms     7.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   263.9±13.97µs    11.2 MB/sec    1.03   270.8±35.05µs    10.9 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.7±0.21ms     5.4 MB/sec    1.00      4.7±0.16ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
