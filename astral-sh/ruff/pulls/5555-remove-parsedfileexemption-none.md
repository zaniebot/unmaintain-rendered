```yaml
number: 5555
title: "Remove `ParsedFileExemption::None`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - suppression
assignees: []
merged: true
base: main
head: charlie/exempt
created_at: 2023-07-06T05:51:48Z
updated_at: 2023-07-06T15:15:48Z
url: https://github.com/astral-sh/ruff/pull/5555
synced_at: 2026-01-12T15:55:18Z
```

# Remove `ParsedFileExemption::None`

---

_@charliermarsh_

## Summary

This is more aligned with the other enums in this module. Should've been changed in a previous refactor, just an oversight.

---

_Label `internal` added by @charliermarsh on 2023-07-06 05:51_

---

_Label `noqa` added by @charliermarsh on 2023-07-06 05:51_

---

_Comment by @github-actions[bot] on 2023-07-06 06:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.02ms     5.2 MB/sec    1.01      7.9±0.02ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1746.3±1.58µs     9.5 MB/sec    1.01   1759.7±3.23µs     9.5 MB/sec
formatter/numpy/globals.py                 1.00    195.7±0.21µs    15.1 MB/sec    1.01    197.6±0.29µs    14.9 MB/sec
formatter/pydantic/types.py                1.00      3.8±0.01ms     6.7 MB/sec    1.00      3.8±0.02ms     6.7 MB/sec
linter/all-rules/large/dataset.py          1.00     13.3±0.04ms     3.1 MB/sec    1.00     13.2±0.02ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.00ms     5.0 MB/sec    1.00      3.3±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    429.0±0.50µs     6.9 MB/sec    1.00    428.8±1.88µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.7±0.01ms     6.1 MB/sec    1.00      6.6±0.01ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1440.6±4.32µs    11.6 MB/sec    1.00   1436.0±2.49µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.3±0.24µs    18.1 MB/sec    1.00    163.4±0.43µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.00ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.07ms     4.4 MB/sec    1.01      9.4±0.10ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1991.7±20.10µs     8.4 MB/sec    1.01      2.0±0.02ms     8.3 MB/sec
formatter/numpy/globals.py                 1.00    218.4±7.09µs    13.5 MB/sec    1.02    222.3±8.52µs    13.3 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.06ms     5.7 MB/sec    1.00      4.5±0.04ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.00     15.6±0.08ms     2.6 MB/sec    1.04     16.2±0.13ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.0 MB/sec    1.02      4.2±0.03ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    428.3±6.58µs     6.9 MB/sec    1.06   452.3±39.10µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.06ms     3.7 MB/sec    1.03      7.2±0.08ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.06ms     5.0 MB/sec    1.04      8.4±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1653.4±11.81µs    10.1 MB/sec    1.04  1720.5±33.81µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    178.5±2.64µs    16.5 MB/sec    1.02    182.1±1.54µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.1 MB/sec    1.03      3.7±0.02ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-07-06 10:47_

---

_@MichaReiser approved on 2023-07-06 12:27_

---

_Merged by @charliermarsh on 2023-07-06 15:15_

---

_Closed by @charliermarsh on 2023-07-06 15:15_

---

_Branch deleted on 2023-07-06 15:15_

---
