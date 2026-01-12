```yaml
number: 3839
title: "Remove `helpers.rs` dependency on `Binding`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/useless-i
created_at: 2023-04-01T02:59:37Z
updated_at: 2023-04-01T03:46:18Z
url: https://github.com/astral-sh/ruff/pull/3839
synced_at: 2026-01-12T15:55:13Z
```

# Remove `helpers.rs` dependency on `Binding`

---

_@charliermarsh_

_No description provided._

---

_Merged by @charliermarsh on 2023-04-01 03:19_

---

_Closed by @charliermarsh on 2023-04-01 03:19_

---

_Branch deleted on 2023-04-01 03:19_

---

_Comment by @github-actions[bot] on 2023-04-01 03:23_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.3±0.21ms     2.5 MB/sec    1.00     16.2±0.26ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.06ms     4.0 MB/sec    1.00      4.1±0.07ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.07   578.2±21.41µs     5.1 MB/sec    1.00    540.3±5.78µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.10ms     3.7 MB/sec    1.00      7.0±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.5±0.09ms     4.8 MB/sec    1.00      8.4±0.10ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1954.1±21.87µs     8.5 MB/sec    1.00  1923.7±29.54µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    210.6±5.18µs    14.0 MB/sec    1.01    212.6±4.45µs    13.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.9±0.06ms     6.5 MB/sec    1.00      3.9±0.06ms     6.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.2±0.10ms     2.7 MB/sec    1.01     15.5±0.19ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.03ms     4.2 MB/sec    1.01      4.0±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    477.9±4.46µs     6.2 MB/sec    1.03   489.9±16.66µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.06ms     3.9 MB/sec    1.01      6.6±0.04ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.05ms     5.1 MB/sec    1.03      8.2±0.10ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1740.9±15.17µs     9.6 MB/sec    1.04  1802.0±14.66µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    188.4±2.29µs    15.7 MB/sec    1.04    195.8±5.49µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.0 MB/sec    1.03      3.8±0.09ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
