```yaml
number: 4450
title: Specialize ConversionFlag
type: pull_request
state: merged
author: youknowone
labels:
  - internal
assignees: []
merged: true
base: main
head: conversion-flag
created_at: 2023-05-16T14:32:46Z
updated_at: 2023-05-16T16:40:50Z
url: https://github.com/astral-sh/ruff/pull/4450
synced_at: 2026-01-12T03:50:03Z
```

# Specialize ConversionFlag

---

_Pull request opened by @youknowone on 2023-05-16 14:32_

_No description provided._

---

_Renamed from "Conversion flag" to "Specialize ConversionFlag" by @youknowone on 2023-05-16 14:38_

---

_Comment by @github-actions[bot] on 2023-05-16 15:09_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.8±0.23ms     2.4 MB/sec    1.00     16.6±0.17ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.1 MB/sec    1.00      4.1±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    502.2±5.45µs     5.9 MB/sec    1.01    507.9±1.23µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.07ms     3.7 MB/sec    1.00      6.9±0.11ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.09ms     5.1 MB/sec    1.00      8.1±0.08ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1737.4±16.17µs     9.6 MB/sec    1.00  1714.9±23.24µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.9±1.57µs    15.1 MB/sec    1.00    195.6±2.53µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.02ms     7.0 MB/sec    1.00      3.6±0.05ms     7.1 MB/sec
parser/large/dataset.py                    1.00      6.4±0.04ms     6.3 MB/sec    1.00      6.4±0.01ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1265.3±17.53µs    13.2 MB/sec    1.00   1263.0±5.41µs    13.2 MB/sec
parser/numpy/globals.py                    1.03    130.4±1.27µs    22.6 MB/sec    1.00    126.7±1.57µs    23.3 MB/sec
parser/pydantic/types.py                   1.01      2.7±0.03ms     9.4 MB/sec    1.00      2.7±0.05ms     9.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     15.4±0.19ms     2.6 MB/sec    1.00     15.1±0.18ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.9±0.04ms     4.3 MB/sec    1.00      3.8±0.04ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    454.7±9.50µs     6.5 MB/sec    1.01    457.2±8.78µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.5±0.11ms     3.9 MB/sec    1.00      6.4±0.11ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.01      7.4±0.08ms     5.5 MB/sec    1.00      7.4±0.07ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1563.7±45.05µs    10.6 MB/sec    1.01  1580.7±24.08µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    175.3±4.47µs    16.8 MB/sec    1.02    179.5±4.46µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.05ms     7.7 MB/sec    1.02      3.4±0.05ms     7.6 MB/sec
parser/large/dataset.py                    1.00      5.9±0.06ms     6.9 MB/sec    1.10      6.5±0.06ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1118.1±17.22µs    14.9 MB/sec    1.09  1214.2±20.90µs    13.7 MB/sec
parser/numpy/globals.py                    1.00    114.3±2.63µs    25.8 MB/sec    1.07    122.0±2.60µs    24.2 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.04ms    10.2 MB/sec    1.08      2.7±0.06ms     9.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/source_code/generator.rs`:1360 on 2023-05-16 15:38_

Nit: Could we add `is_none` helpers to `ConversionFlag` and use them here?

---

_@MichaReiser approved on 2023-05-16 15:38_

---

_@youknowone reviewed on 2023-05-16 15:40_

---

_Review comment by @youknowone on `crates/ruff_python_ast/src/source_code/generator.rs`:1360 on 2023-05-16 15:40_

Sure, actually it is already added in this version 😉 

---

_@youknowone reviewed on 2023-05-16 15:44_

---

_Review comment by @youknowone on `crates/ruff_python_ast/src/source_code/generator.rs`:1360 on 2023-05-16 15:44_

I applied is_none here

---

_Label `internal` added by @MichaReiser on 2023-05-16 16:00_

---

_Merged by @MichaReiser on 2023-05-16 16:00_

---

_Closed by @MichaReiser on 2023-05-16 16:00_

---

_Branch deleted on 2023-05-16 16:40_

---
