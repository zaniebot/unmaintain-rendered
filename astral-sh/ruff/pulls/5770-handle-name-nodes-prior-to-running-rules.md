```yaml
number: 5770
title: Handle name nodes prior to running rules
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/handle
created_at: 2023-07-15T02:12:02Z
updated_at: 2023-07-15T02:42:42Z
url: https://github.com/astral-sh/ruff/pull/5770
synced_at: 2026-01-12T03:30:21Z
```

# Handle name nodes prior to running rules

---

_Pull request opened by @charliermarsh on 2023-07-15 02:12_

## Summary

This is more consistent with other patterns in the Checker. Shouldn't change behavior at all.

---

_Label `internal` added by @charliermarsh on 2023-07-15 02:12_

---

_Merged by @charliermarsh on 2023-07-15 02:21_

---

_Closed by @charliermarsh on 2023-07-15 02:21_

---

_Branch deleted on 2023-07-15 02:21_

---

_Comment by @github-actions[bot] on 2023-07-15 02:26_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.4±0.04ms     4.3 MB/sec    1.00      9.3±0.04ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.01  1876.2±18.50µs     8.9 MB/sec    1.00   1857.9±2.88µs     9.0 MB/sec
formatter/numpy/globals.py                 1.01    210.7±0.30µs    14.0 MB/sec    1.00    209.4±1.26µs    14.1 MB/sec
formatter/pydantic/types.py                1.02      4.1±0.01ms     6.3 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.5±0.16ms     3.0 MB/sec    1.00     13.5±0.08ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    452.8±0.72µs     6.5 MB/sec    1.00    454.0±0.63µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.02ms     4.2 MB/sec    1.02      6.1±0.03ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.1 MB/sec    1.01      6.7±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1463.7±4.04µs    11.4 MB/sec    1.01   1478.3±3.02µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    169.1±0.74µs    17.5 MB/sec    1.00    169.5±0.31µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.1±0.15ms     3.7 MB/sec    1.02     11.4±0.06ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.03ms     7.8 MB/sec    1.02      2.2±0.02ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    231.8±2.48µs    12.7 MB/sec    1.01    233.1±7.76µs    12.7 MB/sec
formatter/pydantic/types.py                1.02      4.8±0.09ms     5.3 MB/sec    1.00      4.7±0.04ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     16.0±0.17ms     2.5 MB/sec    1.00     16.0±0.21ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.04ms     4.0 MB/sec    1.02      4.2±0.03ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    428.3±6.52µs     6.9 MB/sec    1.01    432.8±6.68µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.10ms     3.6 MB/sec    1.02      7.3±0.06ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.11ms     4.9 MB/sec    1.00      8.2±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1679.2±24.34µs     9.9 MB/sec    1.01  1688.1±23.37µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    182.5±3.90µs    16.2 MB/sec    1.00    183.1±2.83µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.05ms     7.0 MB/sec    1.01      3.7±0.06ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
