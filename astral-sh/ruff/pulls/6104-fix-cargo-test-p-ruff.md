```yaml
number: 6104
title: "Fix `cargo test -p ruff`"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: cargo_test_ruff
created_at: 2023-07-26T20:14:04Z
updated_at: 2023-07-26T20:44:54Z
url: https://github.com/astral-sh/ruff/pull/6104
synced_at: 2026-01-12T03:30:22Z
```

# Fix `cargo test -p ruff`

---

_Pull request opened by @konstin on 2023-07-26 20:14_

Previously, this would fail with "the trait bound `SourceLocation: Serialize` is not satisfied".

---

_Review requested from @MichaReiser by @konstin on 2023-07-26 20:14_

---

_@charliermarsh approved on 2023-07-26 20:18_

---

_Comment by @github-actions[bot] on 2023-07-26 20:26_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.6±0.09ms     4.8 MB/sec    1.00      8.6±0.07ms     4.7 MB/sec
formatter/numpy/ctypeslib.py               1.00  1702.2±28.55µs     9.8 MB/sec    1.00  1695.8±24.78µs     9.8 MB/sec
formatter/numpy/globals.py                 1.00    190.9±5.83µs    15.5 MB/sec    1.00    190.7±4.29µs    15.5 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.09ms     7.0 MB/sec    1.00      3.6±0.07ms     7.0 MB/sec
linter/all-rules/large/dataset.py          1.00     12.2±0.09ms     3.3 MB/sec    1.00     12.2±0.12ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.0±0.02ms     5.5 MB/sec    1.00      3.0±0.01ms     5.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    395.2±2.79µs     7.5 MB/sec    1.01    398.4±0.74µs     7.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.6±0.14ms     4.6 MB/sec    1.01      5.6±0.12ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.00      5.9±0.06ms     6.9 MB/sec    1.01      6.0±0.06ms     6.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1229.0±17.69µs    13.5 MB/sec    1.02   1249.5±8.46µs    13.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    134.6±2.42µs    21.9 MB/sec    1.02    136.7±3.66µs    21.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.7±0.06ms     9.5 MB/sec    1.00      2.7±0.04ms     9.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.0±0.07ms     4.1 MB/sec    1.01     10.1±0.09ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1920.7±16.82µs     8.7 MB/sec    1.06      2.0±0.09ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00    213.3±5.46µs    13.8 MB/sec    1.01    215.8±6.46µs    13.7 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.05ms     6.1 MB/sec    1.06      4.5±0.21ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     14.1±0.08ms     2.9 MB/sec    1.00     14.0±0.07ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.03ms     4.5 MB/sec    1.01      3.7±0.05ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    440.2±4.52µs     6.7 MB/sec    1.01    443.1±8.58µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.05ms     4.0 MB/sec    1.00      6.4±0.05ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.06ms     5.6 MB/sec    1.01      7.3±0.15ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1467.8±20.60µs    11.3 MB/sec    1.01  1476.4±13.76µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.2±2.35µs    18.1 MB/sec    1.01    164.4±2.11µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.02ms     8.1 MB/sec    1.02      3.2±0.09ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @MichaReiser on 2023-07-26 20:44_

Whoop. Thank you

---

_Merged by @MichaReiser on 2023-07-26 20:44_

---

_Closed by @MichaReiser on 2023-07-26 20:44_

---

_Branch deleted on 2023-07-26 20:44_

---
