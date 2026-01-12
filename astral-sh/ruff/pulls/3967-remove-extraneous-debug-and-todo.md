```yaml
number: 3967
title: Remove extraneous debug and TODO
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/import-map
created_at: 2023-04-13T22:32:50Z
updated_at: 2023-04-13T23:03:35Z
url: https://github.com/astral-sh/ruff/pull/3967
synced_at: 2026-01-12T15:55:14Z
```

# Remove extraneous debug and TODO

---

_@charliermarsh_

_No description provided._

---

_Merged by @charliermarsh on 2023-04-13 22:45_

---

_Closed by @charliermarsh on 2023-04-13 22:45_

---

_Branch deleted on 2023-04-13 22:45_

---

_Comment by @github-actions[bot] on 2023-04-13 22:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.5±0.04ms     2.8 MB/sec    1.00     14.4±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    459.8±2.43µs     6.4 MB/sec    1.00    456.3±1.01µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.1±0.02ms     4.2 MB/sec    1.00      6.1±0.03ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.01ms     5.6 MB/sec    1.00      7.2±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1637.1±4.13µs    10.2 MB/sec    1.00   1637.8±3.66µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.0±0.45µs    16.4 MB/sec    1.00    180.2±0.48µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.6 MB/sec    1.00      3.3±0.01ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     17.0±0.48ms     2.4 MB/sec    1.00     16.7±0.32ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      4.4±0.16ms     3.8 MB/sec    1.00      4.2±0.08ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.02   515.0±13.28µs     5.7 MB/sec    1.00   506.2±11.43µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.1±0.20ms     3.6 MB/sec    1.00      7.0±0.15ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.4±0.16ms     4.8 MB/sec    1.00      8.4±0.12ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1820.1±34.93µs     9.1 MB/sec    1.01  1830.6±33.97µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.1±5.27µs    15.0 MB/sec    1.01    198.5±6.35µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.9±0.08ms     6.6 MB/sec    1.00      3.8±0.08ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
