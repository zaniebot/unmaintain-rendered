```yaml
number: 3773
title: Set parents even in same-line cases
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/parent
created_at: 2023-03-28T15:41:15Z
updated_at: 2023-03-28T16:43:04Z
url: https://github.com/astral-sh/ruff/pull/3773
synced_at: 2026-01-12T04:39:45Z
```

# Set parents even in same-line cases

---

_Pull request opened by @charliermarsh on 2023-03-28 15:41_

## Summary

It's a little simpler to set the parent statement regardless, and defer to higher-level utilities to skip unneeded work.

---

_Label `internal` added by @charliermarsh on 2023-03-28 15:41_

---

_Comment by @github-actions[bot] on 2023-03-28 15:58_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     18.9±0.47ms     2.1 MB/sec    1.00     18.8±0.36ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.15ms     3.5 MB/sec    1.00      4.7±0.17ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   625.8±29.54µs     4.7 MB/sec    1.00   625.6±24.50µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.1±0.21ms     3.2 MB/sec    1.00      8.0±0.23ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      9.8±0.25ms     4.2 MB/sec    1.01      9.9±0.30ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.05ms     7.8 MB/sec    1.02      2.2±0.07ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.01   261.6±11.14µs    11.3 MB/sec    1.00   259.0±10.30µs    11.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.14ms     5.7 MB/sec    1.02      4.6±0.18ms     5.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.8±0.25ms     2.6 MB/sec    1.00     15.9±0.16ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.08ms     4.1 MB/sec    1.00      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    495.2±5.35µs     6.0 MB/sec    1.01    501.9±8.17µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.07ms     3.8 MB/sec    1.02      6.8±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.09ms     5.0 MB/sec    1.00      8.2±0.08ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1773.9±14.92µs     9.4 MB/sec    1.02  1806.0±24.74µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.8±3.27µs    15.1 MB/sec    1.02    198.1±9.37µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.05ms     6.8 MB/sec    1.00      3.7±0.05ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-03-28 16:09_

---

_Closed by @charliermarsh on 2023-03-28 16:09_

---

_Branch deleted on 2023-03-28 16:09_

---
