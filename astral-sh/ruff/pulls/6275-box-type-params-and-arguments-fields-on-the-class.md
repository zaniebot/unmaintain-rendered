```yaml
number: 6275
title: Box type params and arguments fields on the class definition node
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/box
created_at: 2023-08-02T14:26:29Z
updated_at: 2023-08-02T17:16:26Z
url: https://github.com/astral-sh/ruff/pull/6275
synced_at: 2026-01-12T02:58:30Z
```

# Box type params and arguments fields on the class definition node

---

_Pull request opened by @charliermarsh on 2023-08-02 14:26_

## Summary

This PR boxes the `TypeParams` and `Arguments` fields on the class definition node. These fields are optional and often emitted, and given that class definition is our largest enum variant, we pay the cost of including them for every statement in the AST. Boxing these types reduces the statement size by 40 bytes, which seems like a good tradeoff given how infrequently these are accessed.

## Test Plan

Need to benchmark, but no behavior changes.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-02 14:26_

---

_Review requested from @zanieb by @charliermarsh on 2023-08-02 14:26_

---

_Comment by @github-actions[bot] on 2023-08-02 14:39_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.35ms     3.7 MB/sec    1.04     11.3±0.34ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.11ms     8.1 MB/sec    1.10      2.3±0.20ms     7.4 MB/sec
formatter/numpy/globals.py                 1.00   237.5±12.96µs    12.4 MB/sec    1.04   246.7±16.92µs    12.0 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.19ms     5.7 MB/sec    1.05      4.7±0.22ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.02     15.2±0.46ms     2.7 MB/sec    1.00     15.0±0.58ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.8±0.12ms     4.4 MB/sec    1.00      3.7±0.10ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.04   543.5±26.21µs     5.4 MB/sec    1.00   520.2±23.14µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.23ms     3.8 MB/sec    1.00      6.7±0.23ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.06      8.2±0.27ms     5.0 MB/sec    1.00      7.7±0.22ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1676.5±84.31µs     9.9 MB/sec    1.00  1626.9±62.14µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.5±8.85µs    15.1 MB/sec    1.01   197.2±12.53µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.5±0.13ms     7.3 MB/sec    1.00      3.5±0.13ms     7.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     10.8±0.38ms     3.8 MB/sec    1.00     10.5±0.34ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.1±0.05ms     8.1 MB/sec    1.00      2.0±0.07ms     8.3 MB/sec
formatter/numpy/globals.py                 1.03   230.5±13.45µs    12.8 MB/sec    1.00   224.6±10.09µs    13.1 MB/sec
formatter/pydantic/types.py                1.01      4.5±0.15ms     5.6 MB/sec    1.00      4.5±0.18ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     13.9±0.42ms     2.9 MB/sec    1.01     14.1±0.45ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.11ms     4.6 MB/sec    1.01      3.7±0.11ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   439.6±13.75µs     6.7 MB/sec    1.01   445.8±16.54µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.25ms     4.1 MB/sec    1.00      6.2±0.21ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.23ms     5.4 MB/sec    1.01      7.6±0.26ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1528.5±48.32µs    10.9 MB/sec    1.00  1519.3±62.25µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    169.1±5.72µs    17.5 MB/sec    1.02   172.3±10.00µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.10ms     7.8 MB/sec    1.00      3.3±0.09ms     7.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb approved on 2023-08-02 15:31_

---

_Comment by @charliermarsh on 2023-08-02 16:36_

Improvement in one of the formatter benchmarks, but otherwise no significant changes:

```
formatter/numpy/globals.py
                        time:   [71.550 µs 71.639 µs 71.751 µs]
                        thrpt:  [41.124 MiB/s 41.188 MiB/s 41.239 MiB/s]
                 change:
                        time:   [-0.9797% -0.7028% -0.4437%] (p = 0.00 < 0.05)
                        thrpt:  [+0.4457% +0.7078% +0.9894%]
                        Change within noise threshold.
Found 9 outliers among 100 measurements (9.00%)
  3 (3.00%) low severe
  2 (2.00%) low mild
  4 (4.00%) high severe
formatter/pydantic/types.py
                        time:   [1.6455 ms 1.6470 ms 1.6492 ms]
                        thrpt:  [15.464 MiB/s 15.485 MiB/s 15.499 MiB/s]
                 change:
                        time:   [-0.5555% -0.2812% +0.0352%] (p = 0.06 > 0.05)
                        thrpt:  [-0.0352% +0.2820% +0.5586%]
                        No change in performance detected.
Found 15 outliers among 100 measurements (15.00%)
  3 (3.00%) low severe
  4 (4.00%) low mild
  4 (4.00%) high mild
  4 (4.00%) high severe
formatter/numpy/ctypeslib.py
                        time:   [754.46 µs 755.57 µs 757.70 µs]
                        thrpt:  [21.976 MiB/s 22.038 MiB/s 22.070 MiB/s]
                 change:
                        time:   [-0.3391% -0.0734% +0.2595%] (p = 0.62 > 0.05)
                        thrpt:  [-0.2588% +0.0734% +0.3403%]
                        No change in performance detected.
Found 18 outliers among 100 measurements (18.00%)
  5 (5.00%) low severe
  1 (1.00%) low mild
  7 (7.00%) high mild
  5 (5.00%) high severe
formatter/large/dataset.py
                        time:   [3.8518 ms 3.8527 ms 3.8537 ms]
                        thrpt:  [10.557 MiB/s 10.560 MiB/s 10.562 MiB/s]
                 change:
                        time:   [-2.2882% -2.1679% -2.0436%] (p = 0.00 < 0.05)
                        thrpt:  [+2.0862% +2.2159% +2.3418%]
                        Performance has improved.
Found 10 outliers among 100 measurements (10.00%)
  2 (2.00%) low severe
  3 (3.00%) high mild
  5 (5.00%) high severe

     Running benches/linter.rs (target/release/deps/linter-67964e7f61d24ad0)
Gnuplot not found, using plotters backend
linter/default-rules/numpy/globals.py
                        time:   [55.174 µs 55.218 µs 55.263 µs]
                        thrpt:  [53.394 MiB/s 53.437 MiB/s 53.479 MiB/s]
                 change:
                        time:   [+0.8750% +1.0949% +1.3119%] (p = 0.00 < 0.05)
                        thrpt:  [-1.2949% -1.0830% -0.8674%]
                        Change within noise threshold.
Found 24 outliers among 100 measurements (24.00%)
  5 (5.00%) low severe
  9 (9.00%) low mild
  8 (8.00%) high mild
  2 (2.00%) high severe
linter/default-rules/pydantic/types.py
                        time:   [1.1923 ms 1.1931 ms 1.1939 ms]
                        thrpt:  [21.362 MiB/s 21.375 MiB/s 21.390 MiB/s]
                 change:
                        time:   [+0.2845% +0.4454% +0.6126%] (p = 0.00 < 0.05)
                        thrpt:  [-0.6088% -0.4434% -0.2837%]
                        Change within noise threshold.
Found 11 outliers among 100 measurements (11.00%)
  6 (6.00%) low severe
  2 (2.00%) low mild
  3 (3.00%) high severe
linter/default-rules/numpy/ctypeslib.py
                        time:   [542.94 µs 543.51 µs 544.03 µs]
                        thrpt:  [30.607 MiB/s 30.637 MiB/s 30.669 MiB/s]
                 change:
                        time:   [-0.3515% -0.0710% +0.2210%] (p = 0.63 > 0.05)
                        thrpt:  [-0.2205% +0.0710% +0.3528%]
                        No change in performance detected.
Found 9 outliers among 100 measurements (9.00%)
  8 (8.00%) low mild
  1 (1.00%) high severe
linter/default-rules/large/dataset.py
                        time:   [2.8058 ms 2.8077 ms 2.8095 ms]
                        thrpt:  [14.480 MiB/s 14.490 MiB/s 14.500 MiB/s]
                 change:
                        time:   [+0.4393% +0.5582% +0.6834%] (p = 0.00 < 0.05)
                        thrpt:  [-0.6788% -0.5551% -0.4374%]
                        Change within noise threshold.
Found 8 outliers among 100 measurements (8.00%)
  6 (6.00%) low mild
  1 (1.00%) high mild
  1 (1.00%) high severe

linter/all-rules/numpy/globals.py
                        time:   [119.03 µs 119.23 µs 119.45 µs]
                        thrpt:  [24.702 MiB/s 24.748 MiB/s 24.789 MiB/s]
                 change:
                        time:   [-0.1302% +0.3024% +0.8426%] (p = 0.25 > 0.05)
                        thrpt:  [-0.8356% -0.3014% +0.1304%]
                        No change in performance detected.
Found 13 outliers among 100 measurements (13.00%)
  3 (3.00%) low mild
  3 (3.00%) high mild
  7 (7.00%) high severe
linter/all-rules/pydantic/types.py
                        time:   [2.2943 ms 2.2988 ms 2.3032 ms]
                        thrpt:  [11.073 MiB/s 11.094 MiB/s 11.116 MiB/s]
                 change:
                        time:   [-0.3591% -0.1436% +0.1068%] (p = 0.22 > 0.05)
                        thrpt:  [-0.1067% +0.1438% +0.3604%]
                        No change in performance detected.
Found 1 outliers among 100 measurements (1.00%)
  1 (1.00%) high mild
linter/all-rules/numpy/ctypeslib.py
                        time:   [1.2667 ms 1.2684 ms 1.2703 ms]
                        thrpt:  [13.108 MiB/s 13.128 MiB/s 13.146 MiB/s]
                 change:
                        time:   [-0.1044% +0.0845% +0.2599%] (p = 0.38 > 0.05)
                        thrpt:  [-0.2592% -0.0844% +0.1045%]
                        No change in performance detected.
Found 3 outliers among 100 measurements (3.00%)
  2 (2.00%) high mild
  1 (1.00%) high severe
linter/all-rules/large/dataset.py
                        time:   [5.0160 ms 5.0219 ms 5.0277 ms]
                        thrpt:  [8.0918 MiB/s 8.1011 MiB/s 8.1105 MiB/s]
                 change:
                        time:   [-0.1332% +0.0404% +0.2194%] (p = 0.66 > 0.05)
                        thrpt:  [-0.2189% -0.0403% +0.1333%]
                        No change in performance detected.
```

---

_Merged by @charliermarsh on 2023-08-02 16:47_

---

_Closed by @charliermarsh on 2023-08-02 16:47_

---

_Branch deleted on 2023-08-02 16:47_

---
