```yaml
number: 6577
title: Remove some extraneous newlines in Cargo.toml
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/cargo
created_at: 2023-08-14T23:26:21Z
updated_at: 2023-08-14T23:49:26Z
url: https://github.com/astral-sh/ruff/pull/6577
synced_at: 2026-01-12T15:55:21Z
```

# Remove some extraneous newlines in Cargo.toml

---

_@charliermarsh_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-08-14 23:26_

---

_Merged by @charliermarsh on 2023-08-14 23:39_

---

_Closed by @charliermarsh on 2023-08-14 23:39_

---

_Branch deleted on 2023-08-14 23:39_

---

_Comment by @github-actions[bot] on 2023-08-14 23:43_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.3±0.09ms    12.2 MB/sec    1.02      3.4±0.09ms    12.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   677.5±22.49µs    24.6 MB/sec    1.01   681.6±18.11µs    24.4 MB/sec
formatter/numpy/globals.py                 1.00     66.7±2.32µs    44.2 MB/sec    1.12    74.9±10.43µs    39.4 MB/sec
formatter/pydantic/types.py                1.00  1365.4±82.85µs    18.7 MB/sec    1.00  1371.9±34.81µs    18.6 MB/sec
linter/all-rules/large/dataset.py          1.00     10.0±0.16ms     4.1 MB/sec    1.00     10.0±0.18ms     4.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.04ms     6.2 MB/sec    1.01      2.7±0.08ms     6.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   391.7±21.38µs     7.5 MB/sec    1.00   391.8±10.72µs     7.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.08ms     4.8 MB/sec    1.01      5.3±0.13ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.01      5.3±0.11ms     7.7 MB/sec    1.00      5.2±0.08ms     7.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1155.6±32.99µs    14.4 MB/sec    1.00  1153.3±28.00µs    14.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    141.7±4.05µs    20.8 MB/sec    1.02    145.1±7.37µs    20.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.05ms    10.5 MB/sec    1.00      2.4±0.05ms    10.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.1±0.02ms    13.0 MB/sec    1.01      3.1±0.02ms    12.9 MB/sec
formatter/numpy/ctypeslib.py               1.00    597.9±7.56µs    27.8 MB/sec    1.00   599.7±10.93µs    27.8 MB/sec
formatter/numpy/globals.py                 1.00     58.9±0.64µs    50.1 MB/sec    1.01     59.3±0.78µs    49.7 MB/sec
formatter/pydantic/types.py                1.00  1235.8±20.58µs    20.6 MB/sec    1.00  1239.7±20.86µs    20.6 MB/sec
linter/all-rules/large/dataset.py          1.00     10.1±0.03ms     4.0 MB/sec    1.00     10.0±0.04ms     4.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.01ms     6.1 MB/sec    1.00      2.7±0.01ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    297.9±2.29µs     9.9 MB/sec    1.01    299.6±2.34µs     9.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.1±0.02ms     5.0 MB/sec    1.00      5.1±0.06ms     5.0 MB/sec
linter/default-rules/large/dataset.py      1.00      5.6±0.03ms     7.3 MB/sec    1.00      5.6±0.05ms     7.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1135.7±8.04µs    14.7 MB/sec    1.00   1137.5±9.76µs    14.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    119.3±0.92µs    24.7 MB/sec    1.00    119.3±2.47µs    24.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.02ms    10.5 MB/sec    1.00      2.4±0.02ms    10.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
