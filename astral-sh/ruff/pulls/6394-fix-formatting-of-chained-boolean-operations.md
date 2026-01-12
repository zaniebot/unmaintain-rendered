```yaml
number: 6394
title: Fix formatting of chained boolean operations
type: pull_request
state: merged
author: zanieb
labels:
  - formatter
assignees: []
merged: true
base: main
head: formatter/bool-op
created_at: 2023-08-07T15:49:29Z
updated_at: 2023-08-07T17:30:30Z
url: https://github.com/astral-sh/ruff/pull/6394
synced_at: 2026-01-12T02:52:04Z
```

# Fix formatting of chained boolean operations

---

_Pull request opened by @zanieb on 2023-08-07 15:49_

Closes https://github.com/astral-sh/ruff/issues/6068

These commits are kind of a mess as I did some stumbling around here. 

Unrolls formatting of chained boolean operations to prevent nested grouping which gives us Black-compatible formatting where each boolean operation is on a new line.

---

_Converted to draft by @zanieb on 2023-08-07 15:49_

---

_@zanieb reviewed on 2023-08-07 15:50_

---

_Review comment by @zanieb on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__expression.py.snap`:378 on 2023-08-07 15:50_

Need to fix these

---

_Review comment by @zanieb on `crates/ruff_python_formatter/tests/snapshots/format@expression__boolean_operation.py.snap`:165 on 2023-08-07 15:50_

Yay!

---

_@zanieb reviewed on 2023-08-07 15:50_

---

_Comment by @github-actions[bot] on 2023-08-07 16:22_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.3±0.04ms     4.9 MB/sec    1.00      8.2±0.06ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1629.1±27.23µs    10.2 MB/sec    1.00  1631.1±17.57µs    10.2 MB/sec
formatter/numpy/globals.py                 1.00    182.9±1.05µs    16.1 MB/sec    1.01    185.2±5.69µs    15.9 MB/sec
formatter/pydantic/types.py                1.01      3.5±0.08ms     7.4 MB/sec    1.00      3.4±0.05ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.02     10.6±0.09ms     3.8 MB/sec    1.00     10.4±0.26ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      2.8±0.01ms     6.0 MB/sec    1.00      2.7±0.00ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    380.7±1.07µs     7.8 MB/sec    1.01    386.2±2.84µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.01      4.8±0.05ms     5.3 MB/sec    1.00      4.7±0.02ms     5.4 MB/sec
linter/default-rules/large/dataset.py      1.00      5.3±0.03ms     7.6 MB/sec    1.01      5.4±0.02ms     7.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1133.2±5.32µs    14.7 MB/sec    1.01   1147.7±1.94µs    14.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    129.1±0.52µs    22.9 MB/sec    1.02    131.1±2.21µs    22.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.01ms    10.8 MB/sec    1.02      2.4±0.00ms    10.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     11.6±0.20ms     3.5 MB/sec    1.00     11.5±0.24ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.3±0.04ms     7.4 MB/sec    1.00      2.2±0.05ms     7.5 MB/sec
formatter/numpy/globals.py                 1.00    250.8±7.76µs    11.8 MB/sec    1.02    255.6±7.29µs    11.5 MB/sec
formatter/pydantic/types.py                1.00      4.9±0.10ms     5.2 MB/sec    1.02      5.0±0.10ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.00     15.2±0.22ms     2.7 MB/sec    1.01     15.3±0.22ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.06ms     4.1 MB/sec    1.01      4.1±0.07ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   504.2±10.69µs     5.9 MB/sec    1.02   512.2±10.69µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.13ms     3.7 MB/sec    1.01      7.0±0.11ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.18ms     5.1 MB/sec    1.03      8.3±0.10ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1661.5±33.19µs    10.0 MB/sec    1.02  1689.6±36.11µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.5±5.52µs    15.6 MB/sec    1.02    194.1±4.14µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.06ms     7.2 MB/sec    1.03      3.6±0.08ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @zanieb on `crates/ruff_python_formatter/src/expression/expr_bool_op.rs`:20 on 2023-08-07 16:40_

Unused. Will remove in follow-up pull request.

---

_@zanieb reviewed on 2023-08-07 16:40_

---

_Marked ready for review by @zanieb on 2023-08-07 16:43_

---

_Label `formatter` added by @zanieb on 2023-08-07 16:45_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_bool_op.rs`:82 on 2023-08-07 16:48_

The two matches seem identical. Can we move the logic to a FormatValue { expr } struct that implements Format?

---

_@MichaReiser approved on 2023-08-07 16:49_

---

_Comment by @MichaReiser on 2023-08-07 16:49_

🙏 

---

_@zanieb reviewed on 2023-08-07 16:50_

---

_Review comment by @zanieb on `crates/ruff_python_formatter/src/expression/expr_bool_op.rs`:82 on 2023-08-07 16:50_

Sure!

---

_Merged by @zanieb on 2023-08-07 17:22_

---

_Closed by @zanieb on 2023-08-07 17:22_

---

_Branch deleted on 2023-08-07 17:22_

---
