```yaml
number: 6346
title: "Remove `ruff_python_ast` prefix in fixes.rs"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/comp
created_at: 2023-08-04T16:39:20Z
updated_at: 2023-08-04T17:14:48Z
url: https://github.com/astral-sh/ruff/pull/6346
synced_at: 2026-01-12T15:55:21Z
```

# Remove `ruff_python_ast` prefix in fixes.rs

---

_@charliermarsh_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-08-04 16:39_

---

_@zanieb approved on 2023-08-04 16:44_

---

_Merged by @charliermarsh on 2023-08-04 16:48_

---

_Closed by @charliermarsh on 2023-08-04 16:48_

---

_Branch deleted on 2023-08-04 16:48_

---

_Comment by @github-actions[bot] on 2023-08-04 16:51_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.02ms     4.9 MB/sec    1.00      8.3±0.03ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1612.3±11.74µs    10.3 MB/sec    1.00  1616.0±16.35µs    10.3 MB/sec
formatter/numpy/globals.py                 1.00    171.1±1.24µs    17.2 MB/sec    1.00    171.1±0.33µs    17.2 MB/sec
formatter/pydantic/types.py                1.01      3.5±0.06ms     7.2 MB/sec    1.00      3.5±0.03ms     7.3 MB/sec
linter/all-rules/large/dataset.py          1.01     11.2±0.04ms     3.6 MB/sec    1.00     11.1±0.02ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      2.9±0.01ms     5.8 MB/sec    1.00      2.9±0.01ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    315.3±1.28µs     9.4 MB/sec    1.00    313.9±2.18µs     9.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.2±0.06ms     4.9 MB/sec    1.00      5.1±0.01ms     5.0 MB/sec
linter/default-rules/large/dataset.py      1.01      5.4±0.02ms     7.5 MB/sec    1.00      5.3±0.01ms     7.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1117.0±14.01µs    14.9 MB/sec    1.00   1110.7±1.81µs    15.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    113.4±0.43µs    26.0 MB/sec    1.01    114.2±0.25µs    25.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.4±0.00ms    10.5 MB/sec    1.00      2.4±0.00ms    10.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.07     12.4±0.66ms     3.3 MB/sec    1.00     11.6±0.71ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.07      2.5±0.17ms     6.7 MB/sec    1.00      2.3±0.14ms     7.1 MB/sec
formatter/numpy/globals.py                 1.03   265.9±39.12µs    11.1 MB/sec    1.00   258.0±15.05µs    11.4 MB/sec
formatter/pydantic/types.py                1.00      5.3±0.31ms     4.8 MB/sec    1.01      5.4±0.19ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.07     16.7±0.83ms     2.4 MB/sec    1.00     15.7±0.85ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.4±0.27ms     3.8 MB/sec    1.00      4.2±0.23ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.06   545.1±33.24µs     5.4 MB/sec    1.00   512.0±41.39µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.06      7.9±0.48ms     3.2 MB/sec    1.00      7.5±0.43ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.04      8.7±0.48ms     4.7 MB/sec    1.00      8.3±0.43ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.08  1821.4±83.27µs     9.1 MB/sec    1.00  1694.3±116.39µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.03   216.9±11.12µs    13.6 MB/sec    1.00   210.0±14.94µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.22ms     6.9 MB/sec    1.00      3.7±0.20ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
