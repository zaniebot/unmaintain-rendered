```yaml
number: 6751
title: "Prefer `range_*` edit methods"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/range
created_at: 2023-08-22T03:46:12Z
updated_at: 2023-08-22T16:05:31Z
url: https://github.com/astral-sh/ruff/pull/6751
synced_at: 2026-01-12T15:55:22Z
```

# Prefer `range_*` edit methods

---

_@charliermarsh_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-08-22 03:46_

---

_Comment by @github-actions[bot] on 2023-08-22 04:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.9±0.08ms    10.5 MB/sec    1.01      3.9±0.05ms    10.4 MB/sec
formatter/numpy/ctypeslib.py               1.01   826.2±15.33µs    20.2 MB/sec    1.00   820.5±13.84µs    20.3 MB/sec
formatter/numpy/globals.py                 1.00     84.7±1.33µs    34.8 MB/sec    1.05     88.6±2.02µs    33.3 MB/sec
formatter/pydantic/types.py                1.00  1622.9±29.66µs    15.7 MB/sec    1.00  1619.7±23.85µs    15.7 MB/sec
linter/all-rules/large/dataset.py          1.01     12.0±0.13ms     3.4 MB/sec    1.00     11.9±0.10ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.04ms     5.2 MB/sec    1.00      3.2±0.03ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    458.9±5.26µs     6.4 MB/sec    1.00    455.9±5.62µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.3±0.09ms     4.1 MB/sec    1.00      6.2±0.10ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.04      6.5±0.05ms     6.2 MB/sec    1.00      6.3±0.08ms     6.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1440.0±11.77µs    11.6 MB/sec    1.00  1394.4±16.88µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.9±2.57µs    18.1 MB/sec    1.00    163.0±2.50µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.03      2.9±0.04ms     8.7 MB/sec    1.00      2.8±0.03ms     9.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      3.8±0.24ms    10.8 MB/sec    1.00      3.7±0.08ms    11.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   739.7±26.07µs    22.5 MB/sec    1.00   738.5±21.66µs    22.5 MB/sec
formatter/numpy/globals.py                 1.02     78.1±7.79µs    37.8 MB/sec    1.00     76.5±4.62µs    38.6 MB/sec
formatter/pydantic/types.py                1.00  1507.7±33.13µs    16.9 MB/sec    1.02  1540.0±48.66µs    16.6 MB/sec
linter/all-rules/large/dataset.py          1.03     13.0±0.26ms     3.1 MB/sec    1.00     12.6±0.24ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.7±0.13ms     4.6 MB/sec    1.00      3.6±0.13ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.01   377.9±12.81µs     7.8 MB/sec    1.00   374.9±10.54µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.8±0.27ms     3.7 MB/sec    1.00      6.6±0.16ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.01      7.2±0.15ms     5.6 MB/sec    1.00      7.2±0.17ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1498.4±26.46µs    11.1 MB/sec    1.03  1536.7±50.81µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    155.2±6.43µs    19.0 MB/sec    1.01    156.7±7.34µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.07ms     7.9 MB/sec    1.00      3.2±0.08ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:54 on 2023-08-22 05:45_

Can't you use parent @ 

---

_@MichaReiser approved on 2023-08-22 05:45_

---

_Merged by @charliermarsh on 2023-08-22 15:46_

---

_Closed by @charliermarsh on 2023-08-22 15:46_

---

_Branch deleted on 2023-08-22 15:46_

---
