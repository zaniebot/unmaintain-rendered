```yaml
number: 5478
title: "Fix `unnecessary-encode-utf8` to fix `encode` on parenthesized strings correctly"
type: pull_request
state: merged
author: harupy
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-unnecessary_encode_utf8
created_at: 2023-07-03T13:26:25Z
updated_at: 2023-07-03T14:11:10Z
url: https://github.com/astral-sh/ruff/pull/5478
synced_at: 2026-01-12T03:36:55Z
```

# Fix `unnecessary-encode-utf8` to fix `encode` on parenthesized strings correctly

---

_Pull request opened by @harupy on 2023-07-03 13:26_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #5477

## Test Plan

<!-- How was it tested? -->

New test cases.

---

_Comment by @github-actions[bot] on 2023-07-03 13:37_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.03ms     5.2 MB/sec    1.00      7.9±0.02ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1725.9±2.14µs     9.6 MB/sec    1.00   1731.4±3.58µs     9.6 MB/sec
formatter/numpy/globals.py                 1.00    192.3±0.76µs    15.3 MB/sec    1.00    193.0±1.94µs    15.3 MB/sec
formatter/pydantic/types.py                1.01      3.8±0.06ms     6.7 MB/sec    1.00      3.7±0.01ms     6.8 MB/sec
linter/all-rules/large/dataset.py          1.01     13.6±0.16ms     3.0 MB/sec    1.00     13.5±0.10ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.02ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    424.3±3.92µs     7.0 MB/sec    1.00    425.8±1.14µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.03ms     4.3 MB/sec    1.00      5.9±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      6.7±0.05ms     6.1 MB/sec    1.00      6.6±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1451.1±2.27µs    11.5 MB/sec    1.00   1435.6±7.05µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    162.0±0.91µs    18.2 MB/sec    1.00    160.8±0.59µs    18.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.11ms     4.4 MB/sec    1.02      9.5±0.20ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.03ms     8.3 MB/sec    1.02      2.0±0.04ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00    230.8±6.56µs    12.8 MB/sec    1.01    232.3±9.04µs    12.7 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.05ms     5.8 MB/sec    1.02      4.5±0.06ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     15.6±0.17ms     2.6 MB/sec    1.01     15.7±0.22ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.09ms     4.0 MB/sec    1.00      4.1±0.10ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    503.6±7.72µs     5.9 MB/sec    1.00    504.2±8.98µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.08ms     3.7 MB/sec    1.01      7.0±0.09ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.17ms     5.0 MB/sec    1.00      8.1±0.15ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1731.6±23.40µs     9.6 MB/sec    1.00  1731.9±23.39µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    202.7±6.02µs    14.6 MB/sec    1.00    201.9±3.05µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.04ms     7.0 MB/sec    1.00      3.6±0.04ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-07-03 14:03_

---

_Label `bug` added by @charliermarsh on 2023-07-03 14:10_

---

_Comment by @charliermarsh on 2023-07-03 14:10_

Thanks!

---

_Merged by @charliermarsh on 2023-07-03 14:11_

---

_Closed by @charliermarsh on 2023-07-03 14:11_

---
