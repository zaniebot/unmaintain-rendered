```yaml
number: 6308
title: "More precise invalid expression check for `UP032`"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: more-precise-invalid-check-UP032
created_at: 2023-08-03T15:35:54Z
updated_at: 2023-08-03T16:22:17Z
url: https://github.com/astral-sh/ruff/pull/6308
synced_at: 2026-01-12T15:55:21Z
```

# More precise invalid expression check for `UP032`

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

More precise invalid expression check for `UP032`.

## Test Plan

<!-- How was it tested? -->

New test cases.

---

_@charliermarsh approved on 2023-08-03 15:39_

---

_Merged by @charliermarsh on 2023-08-03 15:49_

---

_Closed by @charliermarsh on 2023-08-03 15:49_

---

_Branch deleted on 2023-08-03 15:51_

---

_Comment by @github-actions[bot] on 2023-08-03 15:52_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     11.0±0.27ms     3.7 MB/sec    1.00     10.8±0.23ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.09ms     7.8 MB/sec    1.00      2.1±0.06ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    242.8±7.59µs    12.2 MB/sec    1.00    243.2±7.81µs    12.1 MB/sec
formatter/pydantic/types.py                1.00      4.6±0.17ms     5.6 MB/sec    1.00      4.6±0.12ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.04     15.3±0.25ms     2.7 MB/sec    1.00     14.7±0.23ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.9±0.06ms     4.3 MB/sec    1.00      3.8±0.09ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.01   535.8±12.63µs     5.5 MB/sec    1.00   529.7±13.97µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.05      7.0±0.14ms     3.6 MB/sec    1.00      6.7±0.11ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.03      8.1±0.14ms     5.0 MB/sec    1.00      7.8±0.12ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1672.1±36.25µs    10.0 MB/sec    1.00  1624.1±41.32µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.04   198.3±11.86µs    14.9 MB/sec    1.00    190.1±6.35µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.5±0.07ms     7.2 MB/sec    1.00      3.4±0.06ms     7.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.5±0.37ms     3.5 MB/sec    1.01     11.6±0.48ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.09ms     7.5 MB/sec    1.03      2.3±0.08ms     7.3 MB/sec
formatter/numpy/globals.py                 1.01   259.0±22.46µs    11.4 MB/sec    1.00   255.2±21.61µs    11.6 MB/sec
formatter/pydantic/types.py                1.00      5.0±0.20ms     5.1 MB/sec    1.00      4.9±0.18ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.02     16.4±0.40ms     2.5 MB/sec    1.00     16.1±0.44ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.3±0.15ms     3.9 MB/sec    1.00      4.3±0.14ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   528.0±17.80µs     5.6 MB/sec    1.00   529.7±25.13µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.5±0.22ms     3.4 MB/sec    1.00      7.2±0.27ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.02      8.9±0.26ms     4.6 MB/sec    1.00      8.7±0.22ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1849.0±57.80µs     9.0 MB/sec    1.00  1784.1±67.77µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    215.5±7.26µs    13.7 MB/sec    1.00    214.2±7.82µs    13.8 MB/sec
linter/default-rules/pydantic/types.py     1.04      4.0±0.29ms     6.4 MB/sec    1.00      3.8±0.10ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
