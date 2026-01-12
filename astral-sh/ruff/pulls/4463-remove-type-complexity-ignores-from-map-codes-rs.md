```yaml
number: 4463
title: "Remove type-complexity ignores from `map_codes.rs`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/types
created_at: 2023-05-17T00:54:33Z
updated_at: 2023-05-17T01:24:01Z
url: https://github.com/astral-sh/ruff/pull/4463
synced_at: 2026-01-12T15:55:15Z
```

# Remove type-complexity ignores from `map_codes.rs`

---

_@charliermarsh_

_No description provided._

---

_Merged by @charliermarsh on 2023-05-17 01:02_

---

_Closed by @charliermarsh on 2023-05-17 01:02_

---

_Branch deleted on 2023-05-17 01:02_

---

_Comment by @github-actions[bot] on 2023-05-17 01:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.5±0.17ms     2.5 MB/sec    1.00     16.3±0.20ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.1 MB/sec    1.00      4.0±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    493.8±6.71µs     6.0 MB/sec    1.01    499.8±6.73µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.07ms     3.8 MB/sec    1.01      6.8±0.09ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.07ms     5.2 MB/sec    1.00      7.9±0.07ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1728.2±13.25µs     9.6 MB/sec    1.00  1703.7±19.62µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.1±2.99µs    15.4 MB/sec    1.00    193.1±3.30µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.05ms     7.2 MB/sec    1.00      3.5±0.04ms     7.2 MB/sec
parser/large/dataset.py                    1.01      6.3±0.07ms     6.4 MB/sec    1.00      6.3±0.06ms     6.5 MB/sec
parser/numpy/ctypeslib.py                  1.00  1236.1±14.26µs    13.5 MB/sec    1.00  1241.6±11.66µs    13.4 MB/sec
parser/numpy/globals.py                    1.02    128.0±1.49µs    23.0 MB/sec    1.00    125.4±2.77µs    23.5 MB/sec
parser/pydantic/types.py                   1.01      2.7±0.03ms     9.4 MB/sec    1.00      2.7±0.03ms     9.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.5±0.09ms     2.5 MB/sec    1.00     16.3±0.10ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.04ms     4.0 MB/sec    1.00      4.2±0.08ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    500.8±6.18µs     5.9 MB/sec    1.00    501.5±5.64µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.06ms     3.7 MB/sec    1.00      6.9±0.05ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.05ms     5.0 MB/sec    1.00      8.2±0.05ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1707.6±13.39µs     9.8 MB/sec    1.01  1728.5±16.32µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.9±2.69µs    14.9 MB/sec    1.02    201.8±7.62µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.05ms     7.0 MB/sec    1.00      3.7±0.03ms     7.0 MB/sec
parser/large/dataset.py                    1.00      6.7±0.04ms     6.1 MB/sec    1.01      6.7±0.04ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1270.6±12.03µs    13.1 MB/sec    1.00  1274.5±12.14µs    13.1 MB/sec
parser/numpy/globals.py                    1.00    131.0±1.98µs    22.5 MB/sec    1.00    131.5±2.61µs    22.4 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.04ms     9.0 MB/sec    1.01      2.9±0.03ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
