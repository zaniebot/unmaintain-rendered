```yaml
number: 3809
title: Use cache in cargo udeps CI
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: use_cache_in_cargo_udeps_ci
created_at: 2023-03-30T11:09:46Z
updated_at: 2023-03-31T15:25:05Z
url: https://github.com/astral-sh/ruff/pull/3809
synced_at: 2026-01-12T04:28:19Z
```

# Use cache in cargo udeps CI

---

_Pull request opened by @konstin on 2023-03-30 11:09_

Checking for unused dependencies is currently the slowest step just after the cargo test runs

---

_Comment by @github-actions[bot] on 2023-03-30 11:21_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.09     16.7±0.55ms     2.4 MB/sec    1.00     15.4±0.40ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.07      4.3±0.21ms     3.8 MB/sec    1.00      4.0±0.26ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01   528.1±13.42µs     5.6 MB/sec    1.00   522.8±16.00µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.21ms     3.9 MB/sec    1.04      6.9±0.19ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.25ms     5.1 MB/sec    1.07      8.6±0.32ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1780.3±50.02µs     9.4 MB/sec    1.06  1892.4±35.31µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.1±7.63µs    14.7 MB/sec    1.03    205.9±4.96µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.11ms     6.9 MB/sec    1.05      3.9±0.06ms     6.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.2±0.22ms     2.5 MB/sec    1.01     16.4±0.20ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.08ms     4.0 MB/sec    1.01      4.2±0.08ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   500.9±12.38µs     5.9 MB/sec    1.05   526.0±19.51µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.08ms     3.7 MB/sec    1.05      7.2±0.22ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.19ms     4.8 MB/sec    1.03      8.7±0.17ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1831.5±35.12µs     9.1 MB/sec    1.02  1872.3±47.82µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.4±8.64µs    15.1 MB/sec    1.03    201.8±7.77µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.05ms     6.6 MB/sec    1.03      4.0±0.09ms     6.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @konstin on 2023-03-30 11:42_

Surprised it isn't more speedup, but it looks like 1-2min at least compared to main

---

_Review requested from @charliermarsh by @konstin on 2023-03-30 11:42_

---

_Review requested from @MichaReiser by @konstin on 2023-03-30 11:42_

---

_@MichaReiser approved on 2023-03-30 11:48_

---

_Merged by @charliermarsh on 2023-03-31 15:07_

---

_Closed by @charliermarsh on 2023-03-31 15:07_

---

_Label `internal` added by @charliermarsh on 2023-03-31 15:07_

---

_Branch deleted on 2023-03-31 15:25_

---
