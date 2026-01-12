```yaml
number: 4478
title: "Move `Insertion` into its own module"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/insertion
created_at: 2023-05-17T20:44:53Z
updated_at: 2023-05-17T21:33:08Z
url: https://github.com/astral-sh/ruff/pull/4478
synced_at: 2026-01-12T15:55:15Z
```

# Move `Insertion` into its own module

---

_@charliermarsh_

## Summary

Also moves most of the insertion-related helpers associated methods on the struct itself.

(No behavioral changes.)


---

_Label `internal` added by @charliermarsh on 2023-05-17 20:44_

---

_Merged by @charliermarsh on 2023-05-17 21:11_

---

_Closed by @charliermarsh on 2023-05-17 21:11_

---

_Branch deleted on 2023-05-17 21:11_

---

_Comment by @github-actions[bot] on 2023-05-17 21:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.9±0.02ms     2.7 MB/sec    1.00     14.9±0.03ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.00ms     4.6 MB/sec    1.00      3.7±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    374.3±1.68µs     7.9 MB/sec    1.00    374.8±1.04µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.01ms     5.5 MB/sec    1.00      7.4±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1554.5±5.66µs    10.7 MB/sec    1.00   1554.6±4.32µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    167.3±0.32µs    17.6 MB/sec    1.00    168.1±0.30µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.6 MB/sec    1.00      3.3±0.00ms     7.7 MB/sec
parser/large/dataset.py                    1.00      6.0±0.00ms     6.8 MB/sec    1.00      6.0±0.00ms     6.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1171.9±1.94µs    14.2 MB/sec    1.01  1180.3±17.60µs    14.1 MB/sec
parser/numpy/globals.py                    1.00    120.8±2.58µs    24.4 MB/sec    1.00    120.8±0.45µs    24.4 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.1 MB/sec    1.01      2.5±0.00ms    10.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     17.3±0.25ms     2.3 MB/sec    1.00     16.9±0.07ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.4±0.08ms     3.8 MB/sec    1.00      4.3±0.02ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    446.2±5.48µs     6.6 MB/sec    1.00    444.8±5.10µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.04ms     3.5 MB/sec    1.00      7.2±0.04ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.6±0.04ms     4.7 MB/sec    1.00      8.5±0.04ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1787.0±10.60µs     9.3 MB/sec    1.00  1780.2±10.74µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    193.8±2.72µs    15.2 MB/sec    1.00    193.1±5.03µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.9±0.02ms     6.6 MB/sec    1.00      3.8±0.05ms     6.6 MB/sec
parser/large/dataset.py                    1.00      7.0±0.03ms     5.8 MB/sec    1.01      7.1±0.08ms     5.7 MB/sec
parser/numpy/ctypeslib.py                  1.00  1326.1±21.11µs    12.6 MB/sec    1.01  1339.4±10.19µs    12.4 MB/sec
parser/numpy/globals.py                    1.00    137.6±1.44µs    21.4 MB/sec    1.00    138.0±2.20µs    21.4 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.01ms     8.7 MB/sec    1.01      3.0±0.02ms     8.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
