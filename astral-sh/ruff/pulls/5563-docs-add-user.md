```yaml
number: 5563
title: "docs: add user"
type: pull_request
state: merged
author: sbrugman
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2023-07-06T14:50:32Z
updated_at: 2023-07-06T16:18:33Z
url: https://github.com/astral-sh/ruff/pull/5563
synced_at: 2026-01-12T03:36:55Z
```

# docs: add user

---

_Pull request opened by @sbrugman on 2023-07-06 14:50_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Adding two repositories at ING Bank using ruff. Demonstrates corporate/industry adoption, e.g. similar to AstraZeneca. 

## Test Plan

Note that the tests failing seems unrelated.

---

_Comment by @github-actions[bot] on 2023-07-06 15:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.1±0.30ms     4.0 MB/sec    1.00     10.1±0.34ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.08ms     7.5 MB/sec    1.02      2.3±0.10ms     7.4 MB/sec
formatter/numpy/globals.py                 1.02   266.4±60.28µs    11.1 MB/sec    1.00   260.0±12.28µs    11.3 MB/sec
formatter/pydantic/types.py                1.00      4.9±0.17ms     5.2 MB/sec    1.02      5.0±0.16ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.00     17.7±0.51ms     2.3 MB/sec    1.02     18.1±0.62ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.4±0.15ms     3.8 MB/sec    1.00      4.3±0.15ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   566.3±24.71µs     5.2 MB/sec    1.01   569.6±21.85µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.7±0.27ms     3.3 MB/sec    1.00      7.7±0.25ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.22ms     4.7 MB/sec    1.05      9.0±0.23ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1834.5±68.18µs     9.1 MB/sec    1.03  1892.5±62.97µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    219.1±8.15µs    13.5 MB/sec    1.03   225.0±14.31µs    13.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.14ms     6.5 MB/sec    1.05      4.1±0.20ms     6.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.2±0.45ms     3.6 MB/sec    1.09     12.2±0.53ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.4±0.12ms     6.9 MB/sec    1.10      2.6±0.17ms     6.3 MB/sec
formatter/numpy/globals.py                 1.00   277.7±19.16µs    10.6 MB/sec    1.07   296.5±18.34µs    10.0 MB/sec
formatter/pydantic/types.py                1.00      5.4±0.34ms     4.7 MB/sec    1.08      5.8±0.35ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.00     20.2±0.66ms     2.0 MB/sec    1.00     20.1±0.68ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      5.3±0.33ms     3.1 MB/sec    1.00      5.1±0.20ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.04   636.8±33.17µs     4.6 MB/sec    1.00   609.8±36.14µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.03      8.9±0.32ms     2.9 MB/sec    1.00      8.7±0.30ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.02     10.3±0.36ms     3.9 MB/sec    1.00     10.1±0.33ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04      2.2±0.11ms     7.5 MB/sec    1.00      2.1±0.10ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.03   254.2±15.33µs    11.6 MB/sec    1.00   247.3±14.15µs    11.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.22ms     5.6 MB/sec    1.00      4.5±0.22ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-07-06 15:50_

---

_Comment by @charliermarsh on 2023-07-06 15:50_

Thanks!

---

_Merged by @charliermarsh on 2023-07-06 15:55_

---

_Closed by @charliermarsh on 2023-07-06 15:55_

---

_Branch deleted on 2023-07-06 15:59_

---
