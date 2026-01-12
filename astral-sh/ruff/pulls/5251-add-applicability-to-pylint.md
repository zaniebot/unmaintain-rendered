```yaml
number: 5251
title: Add Applicability to pylint
type: pull_request
state: merged
author: evanrittenhouse
labels: []
assignees: []
merged: true
base: main
head: applicability_pylint
created_at: 2023-06-21T13:14:47Z
updated_at: 2023-06-21T17:45:13Z
url: https://github.com/astral-sh/ruff/pull/5251
synced_at: 2026-01-12T15:55:18Z
```

# Add Applicability to pylint

---

_@evanrittenhouse_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

Fixes part of #4184 

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-06-21 13:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.6±0.01ms     6.2 MB/sec    1.00      6.5±0.01ms     6.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1359.9±3.43µs    12.2 MB/sec    1.00   1363.9±2.90µs    12.2 MB/sec
formatter/numpy/globals.py                 1.00    131.3±0.13µs    22.5 MB/sec    1.00    130.9±0.62µs    22.5 MB/sec
formatter/pydantic/types.py                1.03      2.7±0.00ms     9.3 MB/sec    1.00      2.7±0.01ms     9.6 MB/sec
linter/all-rules/large/dataset.py          1.01     13.2±0.06ms     3.1 MB/sec    1.00     13.0±0.02ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.00ms     5.0 MB/sec    1.00      3.3±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    431.4±0.84µs     6.8 MB/sec    1.00    426.5±0.82µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.9±0.01ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.01ms     6.1 MB/sec    1.00      6.6±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1468.0±7.93µs    11.3 MB/sec    1.00   1465.9±2.68µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.0±0.29µs    18.0 MB/sec    1.01    166.0±0.27µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.00ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.22     12.8±0.70ms     3.2 MB/sec    1.00     10.5±0.49ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.09      2.4±0.13ms     6.9 MB/sec    1.00      2.2±0.17ms     7.5 MB/sec
formatter/numpy/globals.py                 1.06   239.0±17.81µs    12.3 MB/sec    1.00   226.5±18.67µs    13.0 MB/sec
formatter/pydantic/types.py                1.21      5.2±0.32ms     4.9 MB/sec    1.00      4.3±0.25ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     20.8±0.72ms  2002.4 KB/sec    1.03     21.5±0.79ms  1937.3 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.5±0.28ms     3.0 MB/sec    1.07      5.9±0.30ms     2.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   682.7±40.43µs     4.3 MB/sec    1.04   706.7±42.86µs     4.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.4±0.43ms     2.7 MB/sec    1.05      9.9±0.43ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.00     10.9±0.44ms     3.7 MB/sec    1.01     11.0±0.49ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.4±0.11ms     7.0 MB/sec    1.00      2.3±0.08ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   280.2±17.56µs    10.5 MB/sec    1.08   302.0±23.58µs     9.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.9±0.25ms     5.2 MB/sec    1.07      5.2±0.27ms     4.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-21 17:22_

---

_Closed by @charliermarsh on 2023-06-21 17:22_

---
