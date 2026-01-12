```yaml
number: 4636
title: Only run the playground release job on release
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/playground
created_at: 2023-05-24T15:21:17Z
updated_at: 2023-05-24T16:23:03Z
url: https://github.com/astral-sh/ruff/pull/4636
synced_at: 2026-01-12T03:50:03Z
```

# Only run the playground release job on release

---

_Pull request opened by @charliermarsh on 2023-05-24 15:21_

## Summary

It feels like a poor use of worker capacity to be building the WASM release build on every commit. Let's just run the playground release on _actual_ release.


---

_Merged by @charliermarsh on 2023-05-24 15:48_

---

_Closed by @charliermarsh on 2023-05-24 15:48_

---

_Branch deleted on 2023-05-24 15:48_

---

_Comment by @github-actions[bot] on 2023-05-24 16:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.5±0.50ms     2.5 MB/sec    1.05     17.3±0.63ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.17ms     4.1 MB/sec    1.01      4.1±0.15ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.04   541.1±29.43µs     5.5 MB/sec    1.00   520.1±17.54µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.08      7.7±0.61ms     3.3 MB/sec    1.00      7.2±0.24ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.43ms     5.4 MB/sec    1.06      7.9±0.30ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1700.6±77.03µs     9.8 MB/sec    1.06  1795.0±57.23µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.8±6.55µs    15.1 MB/sec    1.15   225.0±10.26µs    13.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.18ms     7.3 MB/sec    1.10      3.8±0.08ms     6.7 MB/sec
parser/large/dataset.py                    1.00      5.7±0.15ms     7.1 MB/sec    1.07      6.1±0.28ms     6.7 MB/sec
parser/numpy/ctypeslib.py                  1.00  1172.8±53.11µs    14.2 MB/sec    1.05  1230.8±45.21µs    13.5 MB/sec
parser/numpy/globals.py                    1.00    115.6±4.07µs    25.5 MB/sec    1.08   124.5±11.24µs    23.7 MB/sec
parser/pydantic/types.py                   1.00      2.6±0.12ms     9.7 MB/sec    1.05      2.8±0.10ms     9.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.9±0.05ms     2.4 MB/sec    1.00     16.8±0.04ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.02ms     3.8 MB/sec    1.00      4.3±0.06ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    443.3±9.55µs     6.7 MB/sec    1.00    443.1±4.65µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.03ms     3.5 MB/sec    1.00      7.2±0.03ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.5±0.03ms     4.8 MB/sec    1.00      8.4±0.06ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1796.4±9.79µs     9.3 MB/sec    1.00  1799.1±13.41µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.5±1.23µs    14.9 MB/sec    1.00    198.0±3.74µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.02ms     6.6 MB/sec    1.00      3.9±0.09ms     6.6 MB/sec
parser/large/dataset.py                    1.00      6.8±0.02ms     6.0 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00   1285.8±5.83µs    13.0 MB/sec    1.00   1282.6±5.64µs    13.0 MB/sec
parser/numpy/globals.py                    1.01    132.3±1.04µs    22.3 MB/sec    1.00    131.2±0.75µs    22.5 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.01ms     8.9 MB/sec    1.00      2.9±0.01ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
