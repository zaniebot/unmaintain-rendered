```yaml
number: 3816
title: "Use `panic` instead of `unreachable` for invalid arguments"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/panic
created_at: 2023-03-30T15:32:39Z
updated_at: 2023-03-30T16:02:32Z
url: https://github.com/astral-sh/ruff/pull/3816
synced_at: 2026-01-12T15:55:13Z
```

# Use `panic` instead of `unreachable` for invalid arguments

---

_@charliermarsh_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-03-30 15:32_

---

_Merged by @charliermarsh on 2023-03-30 15:40_

---

_Closed by @charliermarsh on 2023-03-30 15:40_

---

_Branch deleted on 2023-03-30 15:40_

---

_Comment by @github-actions[bot] on 2023-03-30 15:44_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.7±0.11ms     2.4 MB/sec    1.00     16.6±0.11ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.13      4.8±1.71ms     3.5 MB/sec    1.00      4.2±0.04ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.24  685.9±369.57µs     4.3 MB/sec    1.00    554.6±1.75µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.1±0.05ms     3.6 MB/sec    1.00      7.1±0.04ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.08ms     4.7 MB/sec    1.00      8.6±0.03ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1947.3±14.04µs     8.6 MB/sec    1.00   1930.3±7.68µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    213.4±0.61µs    13.8 MB/sec    1.02    218.5±5.24µs    13.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.02ms     6.4 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     19.0±0.60ms     2.1 MB/sec    1.00     18.7±0.46ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.9±0.13ms     3.4 MB/sec    1.00      4.9±0.15ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.01   615.0±23.38µs     4.8 MB/sec    1.00   610.3±24.94µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.2±0.27ms     3.1 MB/sec    1.00      8.1±0.23ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.02      9.8±0.30ms     4.1 MB/sec    1.00      9.7±0.21ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.09ms     7.8 MB/sec    1.01      2.2±0.08ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    247.4±8.44µs    11.9 MB/sec    1.01   249.7±15.15µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.14ms     5.6 MB/sec    1.01      4.6±0.19ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
