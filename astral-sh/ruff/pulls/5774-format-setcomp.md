```yaml
number: 5774
title: "Format `SetComp`"
type: pull_request
state: merged
author: lkh42t
labels: []
assignees: []
merged: true
base: main
head: format-expr-set-comp
created_at: 2023-07-15T10:19:52Z
updated_at: 2023-07-16T07:51:13Z
url: https://github.com/astral-sh/ruff/pull/5774
synced_at: 2026-01-12T03:30:21Z
```

# Format `SetComp`

---

_Pull request opened by @lkh42t on 2023-07-15 10:19_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Format `SetComp` like `ListComp`.

## Test Plan

Derived from `ListComp`'s fixture.

---

_Comment by @github-actions[bot] on 2023-07-15 10:29_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.09ms     4.1 MB/sec    1.00      9.9±0.07ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1891.0±2.73µs     8.8 MB/sec    1.00   1882.7±2.96µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    205.1±0.28µs    14.4 MB/sec    1.00    204.9±0.46µs    14.4 MB/sec
formatter/pydantic/types.py                1.01      4.2±0.02ms     6.0 MB/sec    1.00      4.2±0.02ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.1±0.03ms     2.9 MB/sec    1.01     14.2±0.06ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    381.7±1.37µs     7.7 MB/sec    1.01    384.9±2.84µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.02ms     4.1 MB/sec    1.01      6.4±0.05ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.02ms     5.8 MB/sec    1.01      7.1±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1451.0±4.41µs    11.5 MB/sec    1.01   1462.3±2.93µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    155.3±0.59µs    19.0 MB/sec    1.00    155.8±0.80µs    18.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.1 MB/sec    1.00      3.2±0.01ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.19ms     3.7 MB/sec    1.00     10.9±0.11ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.05ms     7.8 MB/sec    1.00      2.1±0.05ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    243.8±4.59µs    12.1 MB/sec    1.01   247.4±10.08µs    11.9 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.06ms     5.5 MB/sec    1.00      4.7±0.06ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.00     16.0±0.15ms     2.5 MB/sec    1.00     16.0±0.22ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.07ms     4.0 MB/sec    1.00      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    505.7±7.77µs     5.8 MB/sec    1.00    506.9±9.93µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.07ms     3.6 MB/sec    1.00      7.2±0.07ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.06ms     5.0 MB/sec    1.00      8.1±0.07ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1721.0±15.46µs     9.7 MB/sec    1.00  1718.8±36.20µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    201.2±3.37µs    14.7 MB/sec    1.00    201.7±6.43µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.03ms     7.0 MB/sec    1.00      3.6±0.03ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-07-15 14:50_

Nice!

---

_Merged by @MichaReiser on 2023-07-15 14:50_

---

_Closed by @MichaReiser on 2023-07-15 14:50_

---

_Branch deleted on 2023-07-16 07:51_

---
