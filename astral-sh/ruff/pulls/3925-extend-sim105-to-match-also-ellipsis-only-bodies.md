```yaml
number: 3925
title: "Extend SIM105 to match also 'Ellipsis only' bodies in exception handlers"
type: pull_request
state: merged
author: leiserfg
labels: []
assignees: []
merged: true
base: main
head: Extend-SIM105
created_at: 2023-04-10T08:31:12Z
updated_at: 2023-04-10T13:55:03Z
url: https://github.com/astral-sh/ruff/pull/3925
synced_at: 2026-01-12T15:55:14Z
```

# Extend SIM105 to match also 'Ellipsis only' bodies in exception handlers

---

_@leiserfg_

_No description provided._

---

_Comment by @github-actions[bot] on 2023-04-10 08:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.05ms     2.9 MB/sec    1.00     14.2±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    459.7±4.62µs     6.4 MB/sec    1.01    464.2±3.10µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.0±0.03ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.2±0.03ms     5.6 MB/sec    1.00      7.2±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1634.4±1.83µs    10.2 MB/sec    1.01   1647.4±7.30µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.5±0.43µs    16.3 MB/sec    1.00    180.9±0.91µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.4±0.03ms     7.6 MB/sec    1.00      3.3±0.00ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.10     20.9±1.30ms  1990.2 KB/sec    1.00     19.0±0.72ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.22      6.0±0.25ms     2.8 MB/sec    1.00      4.9±0.22ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.14   679.2±44.17µs     4.3 MB/sec    1.00   594.6±32.39µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.17      9.9±0.34ms     2.6 MB/sec    1.00      8.4±0.36ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.02     11.7±0.32ms     3.5 MB/sec    1.00     11.5±0.84ms     3.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.4±0.10ms     6.9 MB/sec    1.05      2.5±0.07ms     6.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   243.8±34.55µs    12.1 MB/sec    1.19   290.6±14.54µs    10.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.45ms     5.5 MB/sec    1.17      5.4±0.16ms     4.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-10 13:55_

---

_Closed by @charliermarsh on 2023-04-10 13:55_

---
