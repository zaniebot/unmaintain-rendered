```yaml
number: 5807
title: "Change `pandas-use-of-dot-read-table` rule to emit only when `read_table` is used on CSV data"
type: pull_request
state: merged
author: tjkuson
labels: []
assignees: []
merged: true
base: main
head: pd-read-table
created_at: 2023-07-16T14:17:02Z
updated_at: 2023-07-17T09:39:18Z
url: https://github.com/astral-sh/ruff/pull/5807
synced_at: 2026-01-12T15:55:19Z
```

# Change `pandas-use-of-dot-read-table` rule to emit only when `read_table` is used on CSV data

---

_@tjkuson_

## Summary

Closes #5628 by only emitting if `sep=","`.  Includes documentation (completes the `pandas-vet` ruleset).

Related to #2646.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-07-16 14:27_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.9±0.01ms     4.1 MB/sec    1.00      9.8±0.07ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.01   1914.1±3.36µs     8.7 MB/sec    1.00   1893.5±3.66µs     8.8 MB/sec
formatter/numpy/globals.py                 1.01    207.1±0.41µs    14.2 MB/sec    1.00    205.8±0.88µs    14.3 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.02ms     6.0 MB/sec    1.00      4.2±0.00ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.02ms     3.0 MB/sec    1.01     13.8±0.01ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.02ms     4.8 MB/sec    1.01      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    371.2±1.22µs     7.9 MB/sec    1.02    378.5±1.51µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.02ms     4.1 MB/sec    1.01      6.2±0.25ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.01ms     5.8 MB/sec    1.04      7.3±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1411.5±2.00µs    11.8 MB/sec    1.03   1460.4±4.29µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    147.2±0.24µs    20.0 MB/sec    1.07    157.2±0.31µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.03      3.2±0.01ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.14ms     4.1 MB/sec    1.00      9.8±0.13ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1931.7±34.14µs     8.6 MB/sec    1.01  1949.5±52.62µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    219.6±6.78µs    13.4 MB/sec    1.01    221.2±7.89µs    13.3 MB/sec
formatter/pydantic/types.py                1.01      4.3±0.17ms     6.0 MB/sec    1.00      4.2±0.07ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.9±0.26ms     2.9 MB/sec    1.01     14.0±0.21ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.09ms     4.6 MB/sec    1.01      3.7±0.06ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   444.9±10.25µs     6.6 MB/sec    1.02    452.7±8.13µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.5±1.38ms     3.9 MB/sec    1.00      6.3±0.09ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.09ms     5.7 MB/sec    1.02      7.3±0.12ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1508.1±26.37µs    11.0 MB/sec    1.02  1545.6±25.69µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    172.1±3.68µs    17.1 MB/sec    1.06    182.8±6.84µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.05ms     8.0 MB/sec    1.01      3.2±0.05ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Change `pandas-use-of-dot-read-table` rule logic" to "Change `pandas-use-of-dot-read-table` rule to emit only when `read_table` is used on CSV data" by @tjkuson on 2023-07-16 18:18_

---

_Merged by @charliermarsh on 2023-07-17 01:25_

---

_Closed by @charliermarsh on 2023-07-17 01:25_

---

_Branch deleted on 2023-07-17 09:39_

---
