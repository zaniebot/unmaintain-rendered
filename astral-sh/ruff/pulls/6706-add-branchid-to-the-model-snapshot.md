```yaml
number: 6706
title: "Add `BranchId` to the model snapshot"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/branch-id
created_at: 2023-08-20T15:25:37Z
updated_at: 2023-08-20T15:57:57Z
url: https://github.com/astral-sh/ruff/pull/6706
synced_at: 2026-01-12T02:52:04Z
```

# Add `BranchId` to the model snapshot

---

_Pull request opened by @charliermarsh on 2023-08-20 15:25_

This _probably_ never matters given the set of rules we support and in fact I'm having trouble thinking of a test-case for it, but it's definitely incorrect _not_ to pass on the `BranchId` here.


---

_Label `internal` added by @charliermarsh on 2023-08-20 15:25_

---

_Merged by @charliermarsh on 2023-08-20 15:35_

---

_Closed by @charliermarsh on 2023-08-20 15:35_

---

_Branch deleted on 2023-08-20 15:35_

---

_Comment by @github-actions[bot] on 2023-08-20 15:39_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.9±0.16ms    10.4 MB/sec    1.01      3.9±0.14ms    10.3 MB/sec
formatter/numpy/ctypeslib.py               1.01   824.1±21.68µs    20.2 MB/sec    1.00   814.2±27.73µs    20.5 MB/sec
formatter/numpy/globals.py                 1.00     81.4±2.97µs    36.3 MB/sec    1.03     83.9±6.43µs    35.2 MB/sec
formatter/pydantic/types.py                1.00  1609.8±65.57µs    15.8 MB/sec    1.00  1602.0±70.09µs    15.9 MB/sec
linter/all-rules/large/dataset.py          1.00     11.3±0.51ms     3.6 MB/sec    1.21     13.6±0.37ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.1±0.16ms     5.3 MB/sec    1.12      3.5±0.12ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   485.3±20.40µs     6.1 MB/sec    1.05   509.9±30.63µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.27ms     3.9 MB/sec    1.04      6.8±0.21ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.16ms     6.1 MB/sec    1.04      6.9±0.18ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1520.3±67.09µs    11.0 MB/sec    1.00  1495.5±92.57µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    185.7±7.92µs    15.9 MB/sec    1.00    183.6±8.35µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.07ms     8.2 MB/sec    1.01      3.1±0.08ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      4.4±0.25ms     9.2 MB/sec     1.40      6.2±1.85ms     6.6 MB/sec
formatter/numpy/ctypeslib.py               1.00   947.5±52.90µs    17.6 MB/sec     1.37  1300.9±362.31µs    12.8 MB/sec
formatter/numpy/globals.py                 1.00     98.6±6.82µs    29.9 MB/sec     1.37   135.0±32.78µs    21.9 MB/sec
formatter/pydantic/types.py                1.00  1930.7±113.24µs    13.2 MB/sec    1.36      2.6±0.69ms     9.7 MB/sec
linter/all-rules/large/dataset.py          1.00     17.8±1.27ms     2.3 MB/sec     1.07     18.9±2.04ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.8±0.19ms     3.5 MB/sec     1.00      4.6±0.32ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.02   562.6±25.92µs     5.2 MB/sec     1.00   552.3±25.52µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.05      9.2±0.40ms     2.8 MB/sec     1.00      8.7±0.62ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.38     12.8±2.14ms     3.2 MB/sec     1.00      9.3±0.48ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.47      2.8±0.62ms     6.0 MB/sec     1.00  1903.4±102.27µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   243.6±16.36µs    12.1 MB/sec     1.31   318.3±94.89µs     9.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.3±0.97ms     4.8 MB/sec     1.07      5.7±1.28ms     4.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
