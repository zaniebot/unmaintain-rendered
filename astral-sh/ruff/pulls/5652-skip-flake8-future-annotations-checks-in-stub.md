```yaml
number: 5652
title: Skip flake8-future-annotations checks in stub files
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/future
created_at: 2023-07-10T14:34:59Z
updated_at: 2023-07-10T15:12:57Z
url: https://github.com/astral-sh/ruff/pull/5652
synced_at: 2026-01-12T15:55:19Z
```

# Skip flake8-future-annotations checks in stub files

---

_@charliermarsh_

Closes https://github.com/astral-sh/ruff/issues/5649.

---

_Merged by @charliermarsh on 2023-07-10 14:49_

---

_Closed by @charliermarsh on 2023-07-10 14:49_

---

_Branch deleted on 2023-07-10 14:49_

---

_Comment by @github-actions[bot] on 2023-07-10 14:51_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.5±0.10ms     4.8 MB/sec    1.00      8.4±0.02ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.01   1821.4±3.53µs     9.1 MB/sec    1.00   1805.9±1.93µs     9.2 MB/sec
formatter/numpy/globals.py                 1.01    200.0±0.55µs    14.8 MB/sec    1.00    198.6±0.28µs    14.9 MB/sec
formatter/pydantic/types.py                1.03      4.2±0.01ms     6.1 MB/sec    1.00      4.0±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     14.2±0.07ms     2.9 MB/sec    1.00     14.1±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    368.7±1.02µs     8.0 MB/sec    1.00    363.7±1.48µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.02ms     4.1 MB/sec    1.00      6.2±0.05ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.02ms     5.7 MB/sec    1.00      7.2±0.03ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1478.2±5.54µs    11.3 MB/sec    1.00   1471.9±3.35µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    158.7±0.47µs    18.6 MB/sec    1.00    156.7±0.29µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.00      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.04ms     4.3 MB/sec    1.00      9.4±0.10ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.03ms     8.3 MB/sec    1.00  1994.7±16.18µs     8.3 MB/sec
formatter/numpy/globals.py                 1.00    218.8±2.35µs    13.5 MB/sec    1.00    219.7±5.95µs    13.4 MB/sec
formatter/pydantic/types.py                1.01      4.6±0.05ms     5.6 MB/sec    1.00      4.5±0.03ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.01     15.6±0.14ms     2.6 MB/sec    1.00     15.5±0.15ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.08ms     4.0 MB/sec    1.00      4.1±0.02ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    430.4±8.15µs     6.9 MB/sec    1.00    424.8±4.50µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.07ms     3.6 MB/sec    1.00      7.0±0.05ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.03ms     5.1 MB/sec    1.00      8.1±0.04ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1644.1±14.95µs    10.1 MB/sec    1.00  1651.8±10.02µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    178.8±1.26µs    16.5 MB/sec    1.00    179.2±1.33µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.0 MB/sec    1.00      3.6±0.02ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
