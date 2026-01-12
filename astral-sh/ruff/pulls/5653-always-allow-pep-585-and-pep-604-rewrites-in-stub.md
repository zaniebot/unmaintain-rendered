```yaml
number: 5653
title: Always allow PEP 585 and PEP 604 rewrites in stub files
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/future-annotations
created_at: 2023-07-10T14:43:07Z
updated_at: 2023-07-10T15:09:12Z
url: https://github.com/astral-sh/ruff/pull/5653
synced_at: 2026-01-12T15:55:19Z
```

# Always allow PEP 585 and PEP 604 rewrites in stub files

---

_@charliermarsh_

Closes https://github.com/astral-sh/ruff/issues/5640.

---

_Merged by @charliermarsh on 2023-07-10 14:51_

---

_Closed by @charliermarsh on 2023-07-10 14:51_

---

_Branch deleted on 2023-07-10 14:51_

---

_Comment by @github-actions[bot] on 2023-07-10 14:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.4±0.03ms     4.8 MB/sec    1.00      8.4±0.03ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   1805.1±6.55µs     9.2 MB/sec    1.00   1806.3±4.45µs     9.2 MB/sec
formatter/numpy/globals.py                 1.00    198.4±1.30µs    14.9 MB/sec    1.00    198.6±0.71µs    14.9 MB/sec
formatter/pydantic/types.py                1.01      4.1±0.02ms     6.2 MB/sec    1.00      4.0±0.02ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     14.2±0.06ms     2.9 MB/sec    1.00     14.2±0.07ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.7 MB/sec    1.00      3.6±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    368.3±1.41µs     8.0 MB/sec    1.00    367.5±0.97µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.3±0.02ms     5.6 MB/sec    1.00      7.2±0.02ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1493.3±4.24µs    11.2 MB/sec    1.00   1487.1±8.18µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    159.6±0.21µs    18.5 MB/sec    1.00    159.1±0.28µs    18.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.2±0.01ms     7.9 MB/sec    1.00      3.2±0.02ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.8±0.06ms     4.6 MB/sec    1.02      9.0±0.30ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.01  1871.1±29.03µs     8.9 MB/sec    1.00  1858.2±20.37µs     9.0 MB/sec
formatter/numpy/globals.py                 1.03    200.6±2.97µs    14.7 MB/sec    1.00    193.8±1.49µs    15.2 MB/sec
formatter/pydantic/types.py                1.04      4.2±0.06ms     6.1 MB/sec    1.00      4.1±0.05ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.3±0.44ms     2.7 MB/sec    1.05     16.1±0.41ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.07ms     4.7 MB/sec    1.08      3.8±0.11ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    399.0±3.47µs     7.4 MB/sec    1.05    420.6±5.42µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.13ms     4.0 MB/sec    1.09      7.0±0.17ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.26ms     5.2 MB/sec    1.09      8.4±0.11ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1500.1±18.34µs    11.1 MB/sec    1.11  1658.6±13.98µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.8±1.29µs    17.8 MB/sec    1.09    179.9±2.09µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.03ms     7.8 MB/sec    1.11      3.6±0.06ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
