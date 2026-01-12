```yaml
number: 5017
title: Update CONTRIBUTING.md guide
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/contributing-ii
created_at: 2023-06-12T00:11:42Z
updated_at: 2023-06-12T00:40:52Z
url: https://github.com/astral-sh/ruff/pull/5017
synced_at: 2026-01-12T15:55:17Z
```

# Update CONTRIBUTING.md guide

---

_@charliermarsh_

## Summary

A few things out-of-date here and there.

---

_Label `documentation` added by @charliermarsh on 2023-06-12 00:11_

---

_Merged by @charliermarsh on 2023-06-12 00:20_

---

_Closed by @charliermarsh on 2023-06-12 00:20_

---

_Branch deleted on 2023-06-12 00:21_

---

_Comment by @github-actions[bot] on 2023-06-12 00:24_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.3±0.01ms     6.5 MB/sec    1.00      6.3±0.02ms     6.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   1300.3±3.17µs    12.8 MB/sec    1.00   1298.9±4.52µs    12.8 MB/sec
formatter/numpy/globals.py                 1.00    125.1±1.42µs    23.6 MB/sec    1.00    125.6±1.37µs    23.5 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.01ms    10.0 MB/sec    1.01      2.6±0.01ms     9.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.03ms     3.0 MB/sec    1.01     13.8±0.12ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    418.1±0.90µs     7.1 MB/sec    1.00    416.3±0.80µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.01ms     6.1 MB/sec    1.00      6.7±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1463.1±2.74µs    11.4 MB/sec    1.00   1464.6±2.93µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.9±0.33µs    18.1 MB/sec    1.01    163.8±0.49µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.01      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      9.0±0.10ms     4.5 MB/sec    1.00      8.8±0.09ms     4.6 MB/sec
formatter/numpy/ctypeslib.py               1.02  1779.3±46.27µs     9.4 MB/sec    1.00  1740.8±23.10µs     9.6 MB/sec
formatter/numpy/globals.py                 1.02    167.9±9.83µs    17.6 MB/sec    1.00    164.0±7.51µs    18.0 MB/sec
formatter/pydantic/types.py                1.02      3.6±0.07ms     7.0 MB/sec    1.00      3.5±0.05ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.00     16.3±0.24ms     2.5 MB/sec    1.00     16.4±0.18ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.00      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    498.6±8.20µs     5.9 MB/sec    1.00    496.1±8.43µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.07ms     3.6 MB/sec    1.01      7.1±0.10ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.08ms     4.9 MB/sec    1.00      8.3±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1758.3±25.74µs     9.5 MB/sec    1.00  1765.3±30.68µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.9±3.26µs    14.7 MB/sec    1.00    201.5±4.11µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.05ms     6.8 MB/sec    1.00      3.7±0.04ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
