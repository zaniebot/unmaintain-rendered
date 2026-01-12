```yaml
number: 6513
title: Replace dynamic implicit concatenation detection with parser flag
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/implicit-concat-ii
created_at: 2023-08-11T20:02:53Z
updated_at: 2023-08-14T14:27:18Z
url: https://github.com/astral-sh/ruff/pull/6513
synced_at: 2026-01-12T15:55:21Z
```

# Replace dynamic implicit concatenation detection with parser flag

---

_@charliermarsh_

## Summary

In https://github.com/astral-sh/ruff/pull/6512, we added a flag to the AST to mark implicitly-concatenated string expressions. This PR makes use of that flag to remove the `is_implicit_concatenation` method.

## Test Plan

`cargo test`


---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-11 20:05_

---

_Comment by @github-actions[bot] on 2023-08-11 20:17_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04      3.9±0.03ms    10.5 MB/sec    1.00      3.7±0.02ms    10.9 MB/sec
formatter/numpy/ctypeslib.py               1.03    749.6±6.04µs    22.2 MB/sec    1.00   730.0±15.85µs    22.8 MB/sec
formatter/numpy/globals.py                 1.03     76.8±0.76µs    38.4 MB/sec    1.00     74.9±0.74µs    39.4 MB/sec
formatter/pydantic/types.py                1.04  1540.9±11.61µs    16.6 MB/sec    1.00  1486.3±11.07µs    17.2 MB/sec
linter/all-rules/large/dataset.py          1.00     10.7±0.05ms     3.8 MB/sec    1.00     10.6±0.07ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.00ms     5.7 MB/sec    1.00      2.9±0.02ms     5.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    329.4±0.95µs     9.0 MB/sec    1.00    327.3±1.66µs     9.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.5±0.02ms     4.6 MB/sec    1.00      5.5±0.02ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.01      5.7±0.02ms     7.2 MB/sec    1.00      5.6±0.02ms     7.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1201.7±2.06µs    13.9 MB/sec    1.00   1190.2±5.35µs    14.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    124.9±0.53µs    23.6 MB/sec    1.01    126.2±0.27µs    23.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.5±0.01ms    10.0 MB/sec    1.00      2.5±0.02ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      4.3±0.09ms     9.4 MB/sec    1.00      4.3±0.06ms     9.6 MB/sec
formatter/numpy/ctypeslib.py               1.02   839.8±10.78µs    19.8 MB/sec    1.00   822.1±12.85µs    20.3 MB/sec
formatter/numpy/globals.py                 1.04     86.5±1.81µs    34.1 MB/sec    1.00     83.5±1.79µs    35.3 MB/sec
formatter/pydantic/types.py                1.02  1741.8±29.46µs    14.6 MB/sec    1.00  1703.1±43.37µs    15.0 MB/sec
linter/all-rules/large/dataset.py          1.01     13.0±0.16ms     3.1 MB/sec    1.00     12.9±0.14ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.6±0.06ms     4.6 MB/sec    1.00      3.5±0.04ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    443.8±7.85µs     6.6 MB/sec    1.00    441.8±8.72µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.8±0.14ms     3.8 MB/sec    1.00      6.7±0.07ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.07ms     5.8 MB/sec    1.01      7.0±0.08ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1499.8±16.12µs    11.1 MB/sec    1.00  1490.9±16.91µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    174.0±3.91µs    17.0 MB/sec    1.00    174.3±3.03µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.2 MB/sec    1.02      3.2±0.04ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `internal` added by @charliermarsh on 2023-08-12 03:40_

---

_@MichaReiser approved on 2023-08-14 08:42_

Can you rebase on main to get the latest benchmark numbers? I now excluded the parse and lex times, so we should get a better picture of the cost relative to the formatting time.

---

_Comment by @charliermarsh on 2023-08-14 14:24_

@MichaReiser - Updated, but the change seems the same? 🤔 

---

_Merged by @charliermarsh on 2023-08-14 14:27_

---

_Closed by @charliermarsh on 2023-08-14 14:27_

---

_Branch deleted on 2023-08-14 14:27_

---
