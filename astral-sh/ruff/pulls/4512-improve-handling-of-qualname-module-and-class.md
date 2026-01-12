```yaml
number: 4512
title: "Improve handling of `__qualname__`, `__module__`, and `__class__`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/qualname
created_at: 2023-05-18T21:41:45Z
updated_at: 2023-05-20T03:25:00Z
url: https://github.com/astral-sh/ruff/pull/4512
synced_at: 2026-01-12T03:50:03Z
```

# Improve handling of `__qualname__`, `__module__`, and `__class__`

---

_Pull request opened by @charliermarsh on 2023-05-18 21:41_

## Summary

Removing some special-casing from `Checker`, and improve handling in a few cases (all the updated test cases were "wrong" before as compared to observed runtime behavior).

---

_Renamed from "Improve handling of __qualname__, __module__, and __class__" to "Improve handling of `__qualname__`, `__module__`, and `__class__`" by @charliermarsh on 2023-05-18 21:42_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-18 21:48_

---

_Review requested from @konstin by @charliermarsh on 2023-05-18 21:48_

---

_Comment by @github-actions[bot] on 2023-05-18 21:56_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.9±0.15ms     2.4 MB/sec    1.00     16.9±0.10ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.06ms     4.1 MB/sec    1.00      4.1±0.01ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    504.4±2.53µs     5.8 MB/sec    1.00    505.2±2.38µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.03ms     3.6 MB/sec    1.04      7.3±0.30ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.03ms     5.1 MB/sec    1.00      8.0±0.03ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1732.2±11.98µs     9.6 MB/sec    1.03  1780.5±111.24µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.2±1.22µs    15.4 MB/sec    1.01    193.7±1.62µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.1 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
parser/large/dataset.py                    1.00      6.4±0.03ms     6.3 MB/sec    1.01      6.5±0.03ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00   1265.0±3.63µs    13.2 MB/sec    1.01   1278.7±5.16µs    13.0 MB/sec
parser/numpy/globals.py                    1.00    129.1±0.79µs    22.8 MB/sec    1.02    131.7±0.56µs    22.4 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.01ms     9.3 MB/sec    1.01      2.8±0.01ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.06     18.5±0.30ms     2.2 MB/sec    1.00     17.4±0.25ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.06      4.6±0.10ms     3.6 MB/sec    1.00      4.3±0.08ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.03    514.7±7.76µs     5.7 MB/sec    1.00    498.6±7.59µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.09      7.8±0.25ms     3.3 MB/sec    1.00      7.1±0.10ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.10      9.2±0.13ms     4.4 MB/sec    1.00      8.3±0.10ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05  1865.8±20.02µs     8.9 MB/sec    1.00  1781.2±57.82µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    206.3±5.33µs    14.3 MB/sec    1.00   206.4±30.14µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.03      4.1±0.06ms     6.3 MB/sec    1.00      4.0±0.44ms     6.4 MB/sec
parser/large/dataset.py                    1.02      6.9±0.06ms     5.9 MB/sec    1.00      6.8±0.06ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.02  1318.2±13.63µs    12.6 MB/sec    1.00  1288.9±17.29µs    12.9 MB/sec
parser/numpy/globals.py                    1.03    136.4±4.40µs    21.6 MB/sec    1.00    133.1±2.38µs    22.2 MB/sec
parser/pydantic/types.py                   1.02      2.9±0.03ms     8.7 MB/sec    1.00      2.9±0.03ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-05-19 06:56_

---

_@konstin approved on 2023-05-19 08:25_

---

_Merged by @charliermarsh on 2023-05-20 03:03_

---

_Closed by @charliermarsh on 2023-05-20 03:03_

---

_Branch deleted on 2023-05-20 03:03_

---
