```yaml
number: 6241
title: Bump version to 0.0.282
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/bump
created_at: 2023-08-01T13:01:11Z
updated_at: 2023-08-01T13:33:03Z
url: https://github.com/astral-sh/ruff/pull/6241
synced_at: 2026-01-12T15:55:21Z
```

# Bump version to 0.0.282

---

_@charliermarsh_

_No description provided._

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-01 13:03_

---

_Merged by @charliermarsh on 2023-08-01 13:21_

---

_Closed by @charliermarsh on 2023-08-01 13:21_

---

_Branch deleted on 2023-08-01 13:21_

---

_Comment by @github-actions[bot] on 2023-08-01 13:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.1±0.12ms     4.0 MB/sec    1.00     10.1±0.07ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.1±0.06ms     8.0 MB/sec    1.00      2.0±0.02ms     8.3 MB/sec
formatter/numpy/globals.py                 1.02    230.5±7.63µs    12.8 MB/sec    1.00    224.9±2.36µs    13.1 MB/sec
formatter/pydantic/types.py                1.04      4.5±0.14ms     5.7 MB/sec    1.00      4.3±0.10ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.02     14.0±0.17ms     2.9 MB/sec    1.00     13.7±0.09ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.8 MB/sec    1.00      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    475.1±1.55µs     6.2 MB/sec    1.00    472.7±1.19µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.4±0.30ms     4.0 MB/sec    1.00      6.2±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.3±0.03ms     5.6 MB/sec    1.00      7.2±0.07ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1518.0±21.61µs    11.0 MB/sec    1.00   1504.1±7.35µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.06    172.7±8.47µs    17.1 MB/sec    1.00    163.3±0.28µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.02ms     7.9 MB/sec    1.00      3.2±0.05ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.10ms     4.1 MB/sec    1.01      9.9±0.11ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1912.1±24.50µs     8.7 MB/sec    1.01  1935.7±28.56µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    212.0±4.44µs    13.9 MB/sec    1.02    215.5±6.99µs    13.7 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.05ms     6.1 MB/sec    1.02      4.3±0.06ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.01     13.5±0.15ms     3.0 MB/sec    1.00     13.4±0.12ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.03ms     4.7 MB/sec    1.01      3.6±0.03ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    434.4±7.38µs     6.8 MB/sec    1.00    436.5±7.89µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.06ms     4.3 MB/sec    1.01      6.0±0.07ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.07ms     5.6 MB/sec    1.01      7.3±0.08ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1482.9±44.72µs    11.2 MB/sec    1.02  1513.4±19.99µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.9±2.78µs    17.8 MB/sec    1.02    168.6±5.65µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.03ms     8.0 MB/sec    1.02      3.2±0.03ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
