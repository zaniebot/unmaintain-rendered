```yaml
number: 3838
title: Move f-string identification into rule module
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/useless
created_at: 2023-04-01T02:56:57Z
updated_at: 2023-04-01T03:21:57Z
url: https://github.com/astral-sh/ruff/pull/3838
synced_at: 2026-01-12T04:28:19Z
```

# Move f-string identification into rule module

---

_Pull request opened by @charliermarsh on 2023-04-01 02:56_

_No description provided._

---

_Comment by @github-actions[bot] on 2023-04-01 03:09_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.8±0.04ms     2.8 MB/sec    1.00     14.7±0.08ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.00ms     4.4 MB/sec    1.00      3.7±0.01ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.01    406.7±1.86µs     7.3 MB/sec    1.00    404.0±2.53µs     7.3 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.4±0.01ms     4.0 MB/sec    1.00      6.3±0.02ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.01      8.0±0.01ms     5.1 MB/sec    1.00      7.9±0.04ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1766.3±6.51µs     9.4 MB/sec    1.00   1756.8±8.26µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    184.3±0.38µs    16.0 MB/sec    1.00    182.5±0.67µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.01ms     6.9 MB/sec    1.00      3.6±0.02ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.4±1.12ms     2.5 MB/sec    1.00     16.2±0.20ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.09ms     4.0 MB/sec    1.01      4.2±0.13ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    496.7±6.79µs     5.9 MB/sec    1.01    499.9±9.28µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.11ms     3.7 MB/sec    1.01      6.9±0.11ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.10ms     4.8 MB/sec    1.01      8.5±0.09ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1840.7±24.54µs     9.0 MB/sec    1.01  1865.6±28.35µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.4±4.45µs    15.0 MB/sec    1.02    201.1±7.30µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.06ms     6.6 MB/sec    1.00      3.9±0.08ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-01 03:10_

---

_Closed by @charliermarsh on 2023-04-01 03:10_

---

_Branch deleted on 2023-04-01 03:10_

---
