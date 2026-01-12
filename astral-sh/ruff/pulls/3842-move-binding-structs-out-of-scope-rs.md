```yaml
number: 3842
title: "Move `Binding` structs out of `scope.rs`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/binding
created_at: 2023-04-01T03:23:40Z
updated_at: 2023-04-01T04:11:52Z
url: https://github.com/astral-sh/ruff/pull/3842
synced_at: 2026-01-12T04:28:19Z
```

# Move `Binding` structs out of `scope.rs`

---

_Pull request opened by @charliermarsh on 2023-04-01 03:23_

_No description provided._

---

_Merged by @charliermarsh on 2023-04-01 03:49_

---

_Closed by @charliermarsh on 2023-04-01 03:49_

---

_Branch deleted on 2023-04-01 03:49_

---

_Comment by @github-actions[bot] on 2023-04-01 03:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     18.0±0.61ms     2.3 MB/sec    1.00     17.7±0.46ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.6±0.17ms     3.7 MB/sec    1.00      4.5±0.27ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   572.9±21.11µs     5.2 MB/sec    1.02   583.8±24.35µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.6±0.47ms     3.4 MB/sec    1.05      7.9±0.21ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.8±0.24ms     4.6 MB/sec    1.03      9.1±0.24ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1985.5±81.15µs     8.4 MB/sec    1.03      2.0±0.08ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    238.5±8.60µs    12.4 MB/sec    1.04    247.1±9.41µs    11.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.16ms     6.2 MB/sec    1.02      4.2±0.13ms     6.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.5±0.12ms     2.6 MB/sec    1.00     15.5±0.07ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.1 MB/sec    1.00      4.1±0.02ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    423.4±5.80µs     7.0 MB/sec    1.00    423.0±6.74µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.02ms     3.8 MB/sec    1.01      6.7±0.03ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.04ms     5.0 MB/sec    1.01      8.2±0.02ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1765.8±9.29µs     9.4 MB/sec    1.01   1792.1±8.40µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    186.5±9.88µs    15.8 MB/sec    1.00    186.4±3.83µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     6.9 MB/sec    1.02      3.8±0.10ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
