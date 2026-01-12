```yaml
number: 6699
title: "Add doc for `E999`"
type: pull_request
state: merged
author: harupy
labels:
  - documentation
assignees: []
merged: true
base: main
head: E999-doc
created_at: 2023-08-20T07:23:32Z
updated_at: 2023-08-20T14:27:04Z
url: https://github.com/astral-sh/ruff/pull/6699
synced_at: 2026-01-12T15:55:22Z
```

# Add doc for `E999`

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

_Comment by @github-actions[bot] on 2023-08-20 07:35_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      3.6±0.15ms    11.2 MB/sec    1.00      3.6±0.17ms    11.3 MB/sec
formatter/numpy/ctypeslib.py               1.01   758.8±36.97µs    21.9 MB/sec    1.00  754.7±102.05µs    22.1 MB/sec
formatter/numpy/globals.py                 1.01     78.2±6.67µs    37.8 MB/sec    1.00     77.4±4.91µs    38.1 MB/sec
formatter/pydantic/types.py                1.00  1491.9±65.40µs    17.1 MB/sec    1.00  1496.0±70.60µs    17.0 MB/sec
linter/all-rules/large/dataset.py          1.02     12.2±0.42ms     3.3 MB/sec    1.00     12.0±0.38ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      3.3±0.16ms     5.0 MB/sec    1.00      3.2±0.10ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   477.7±15.43µs     6.2 MB/sec    1.00   476.2±21.98µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.20ms     4.0 MB/sec    1.03      6.5±0.27ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.01      6.5±0.25ms     6.3 MB/sec    1.00      6.4±0.22ms     6.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1419.0±62.18µs    11.7 MB/sec    1.00  1384.2±50.26µs    12.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   175.4±12.22µs    16.8 MB/sec    1.03   181.4±10.11µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.9±0.11ms     8.7 MB/sec    1.00      2.9±0.15ms     8.8 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.04      4.7±0.29ms     8.7 MB/sec     1.00      4.5±0.19ms     9.0 MB/sec
formatter/numpy/ctypeslib.py               1.01   943.7±41.98µs    17.6 MB/sec     1.00   930.4±31.89µs    17.9 MB/sec
formatter/numpy/globals.py                 1.00     97.8±5.91µs    30.2 MB/sec     1.00     97.4±6.60µs    30.3 MB/sec
formatter/pydantic/types.py                1.03  1930.2±106.98µs    13.2 MB/sec    1.00  1882.6±88.82µs    13.5 MB/sec
linter/all-rules/large/dataset.py          1.00     16.2±0.38ms     2.5 MB/sec     1.02     16.5±0.54ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.21ms     3.8 MB/sec     1.03      4.5±0.23ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   541.7±22.73µs     5.4 MB/sec     1.01   548.3±22.70µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.6±0.31ms     3.0 MB/sec     1.01      8.7±0.31ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.02      9.2±0.45ms     4.4 MB/sec     1.00      9.0±0.25ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1904.8±72.39µs     8.7 MB/sec     1.00  1907.0±57.88µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.02   220.3±14.41µs    13.4 MB/sec     1.00   216.1±11.47µs    13.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.20ms     6.3 MB/sec     1.00      4.1±0.21ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-08-20 14:00_

---

_Merged by @charliermarsh on 2023-08-20 14:14_

---

_Closed by @charliermarsh on 2023-08-20 14:14_

---
