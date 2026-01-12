```yaml
number: 5161
title: Add Applicability to isort
type: pull_request
state: merged
author: evanrittenhouse
labels: []
assignees: []
merged: true
base: main
head: applicability_isort
created_at: 2023-06-17T15:53:34Z
updated_at: 2023-06-17T19:29:11Z
url: https://github.com/astral-sh/ruff/pull/5161
synced_at: 2026-01-12T15:55:17Z
```

# Add Applicability to isort

---

_@evanrittenhouse_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes some of #4184.

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-06-17 16:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.1±0.14ms     6.6 MB/sec    1.02      6.3±0.29ms     6.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1288.5±41.04µs    12.9 MB/sec    1.05  1350.6±77.28µs    12.3 MB/sec
formatter/numpy/globals.py                 1.00    126.2±6.51µs    23.4 MB/sec    1.09   137.9±13.39µs    21.4 MB/sec
formatter/pydantic/types.py                1.00      2.5±0.07ms    10.1 MB/sec    1.11      2.8±0.14ms     9.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.8±0.63ms     3.0 MB/sec    1.00     13.7±0.47ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.07ms     5.2 MB/sec    1.02      3.3±0.10ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   460.1±37.04µs     6.4 MB/sec    1.00   458.8±32.75µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.36ms     4.2 MB/sec    1.01      6.1±0.36ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.16ms     6.2 MB/sec    1.01      6.6±0.16ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1422.8±45.55µs    11.7 MB/sec    1.09  1556.3±111.36µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.6±7.08µs    17.9 MB/sec    1.07   175.5±13.11µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.04      3.2±0.20ms     8.1 MB/sec    1.00      3.1±0.13ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.6±0.08ms     5.3 MB/sec    1.13      8.6±0.07ms     4.7 MB/sec
formatter/numpy/ctypeslib.py               1.00  1568.6±24.67µs    10.6 MB/sec    1.10  1728.5±20.38µs     9.6 MB/sec
formatter/numpy/globals.py                 1.00    151.9±3.18µs    19.4 MB/sec    1.09   164.8±12.10µs    17.9 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.04ms     8.1 MB/sec    1.12      3.5±0.04ms     7.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.16ms     2.6 MB/sec    1.00     15.6±0.11ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.07ms     4.0 MB/sec    1.00      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    490.7±6.51µs     6.0 MB/sec    1.00    490.3±8.24µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.06ms     3.7 MB/sec    1.00      6.9±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.04ms     5.0 MB/sec    1.01      8.2±0.05ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1735.4±15.22µs     9.6 MB/sec    1.00  1741.4±24.93µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.6±6.44µs    14.8 MB/sec    1.00    199.6±2.60µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.9 MB/sec    1.01      3.7±0.03ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/isort/rules/add_required_imports.rs`:121 on 2023-06-17 16:05_

Let's make this automatic I think.

---

_@charliermarsh reviewed on 2023-06-17 16:05_

---

_Merged by @charliermarsh on 2023-06-17 19:08_

---

_Closed by @charliermarsh on 2023-06-17 19:08_

---

_Branch deleted on 2023-06-17 19:22_

---
