```yaml
number: 6479
title: "Add `PT008` and `PT009` docs"
type: pull_request
state: merged
author: harupy
labels:
  - documentation
assignees: []
merged: true
base: main
head: PT008-PT009
created_at: 2023-08-10T13:37:19Z
updated_at: 2023-08-12T04:18:18Z
url: https://github.com/astral-sh/ruff/pull/6479
synced_at: 2026-01-12T02:52:04Z
```

# Add `PT008` and `PT009` docs

---

_Pull request opened by @harupy on 2023-08-10 13:37_

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

_Comment by @github-actions[bot] on 2023-08-10 14:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.05      9.5±0.11ms     4.3 MB/sec    1.00      9.0±0.25ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.06  1879.8±22.29µs     8.9 MB/sec    1.00  1767.4±43.26µs     9.4 MB/sec
formatter/numpy/globals.py                 1.07    212.4±4.74µs    13.9 MB/sec    1.00    198.2±6.19µs    14.9 MB/sec
formatter/pydantic/types.py                1.07      4.0±0.05ms     6.4 MB/sec    1.00      3.7±0.10ms     6.8 MB/sec
linter/all-rules/large/dataset.py          1.12     12.3±0.11ms     3.3 MB/sec    1.00     11.0±0.25ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.09      3.2±0.04ms     5.2 MB/sec    1.00      2.9±0.06ms     5.7 MB/sec
linter/all-rules/numpy/globals.py          1.09    445.0±5.80µs     6.6 MB/sec    1.00    409.0±9.97µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.22      6.2±0.05ms     4.1 MB/sec    1.00      5.1±0.12ms     5.0 MB/sec
linter/default-rules/large/dataset.py      1.12      6.3±0.05ms     6.4 MB/sec    1.00      5.7±0.09ms     7.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.10  1360.0±17.41µs    12.2 MB/sec    1.00  1233.2±21.49µs    13.5 MB/sec
linter/default-rules/numpy/globals.py      1.13    154.9±2.02µs    19.0 MB/sec    1.00    137.5±3.21µs    21.5 MB/sec
linter/default-rules/pydantic/types.py     1.12      2.8±0.03ms     9.0 MB/sec    1.00      2.5±0.06ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.0±0.09ms     4.1 MB/sec    1.00      9.9±0.10ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.01  1920.8±44.26µs     8.7 MB/sec    1.00  1897.1±38.72µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    211.4±3.94µs    14.0 MB/sec    1.00    210.9±7.09µs    14.0 MB/sec
formatter/pydantic/types.py                1.01      4.2±0.06ms     6.0 MB/sec    1.00      4.2±0.05ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.02     12.7±0.11ms     3.2 MB/sec    1.00     12.5±0.13ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.5±0.04ms     4.8 MB/sec    1.00      3.4±0.03ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    432.7±6.78µs     6.8 MB/sec    1.00    426.4±7.57µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.13      6.5±0.07ms     3.9 MB/sec    1.00      5.8±0.07ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.02      6.8±0.06ms     6.0 MB/sec    1.00      6.7±0.08ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1430.5±16.82µs    11.6 MB/sec    1.00  1388.3±17.26µs    12.0 MB/sec
linter/default-rules/numpy/globals.py      1.04    164.8±2.34µs    17.9 MB/sec    1.00    158.7±2.13µs    18.6 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.0±0.04ms     8.4 MB/sec    1.00      3.0±0.06ms     8.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-08-11 01:18_

---

_Merged by @charliermarsh on 2023-08-12 03:44_

---

_Closed by @charliermarsh on 2023-08-12 03:44_

---

_Branch deleted on 2023-08-12 04:18_

---
