```yaml
number: 4846
title: Modify semantic model API to push bindings upon creation
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/add-binding
created_at: 2023-06-04T02:20:49Z
updated_at: 2023-06-04T02:57:55Z
url: https://github.com/astral-sh/ruff/pull/4846
synced_at: 2026-01-12T15:55:16Z
```

# Modify semantic model API to push bindings upon creation

---

_@charliermarsh_

## Summary

Follow-up to some feedback in #4819.

My local benchmarks reported a meaningful performance improvement, but I'm not sure why that would be:

```
linter/default-rules/numpy/globals.py
                        time:   [77.086 µs 77.117 µs 77.159 µs]
                        thrpt:  [38.241 MiB/s 38.262 MiB/s 38.278 MiB/s]
                 change:
                        time:   [-0.0523% +0.1805% +0.3988%] (p = 0.12 > 0.05)
                        thrpt:  [-0.3972% -0.1801% +0.0523%]
                        No change in performance detected.
Found 10 outliers among 100 measurements (10.00%)
  1 (1.00%) low severe
  5 (5.00%) high mild
  4 (4.00%) high severe
linter/default-rules/pydantic/types.py
                        time:   [1.4842 ms 1.4845 ms 1.4848 ms]
                        thrpt:  [17.176 MiB/s 17.180 MiB/s 17.183 MiB/s]
                 change:
                        time:   [-0.2148% -0.1180% +0.0003%] (p = 0.02 < 0.05)
                        thrpt:  [-0.0003% +0.1181% +0.2152%]
                        Change within noise threshold.
Found 6 outliers among 100 measurements (6.00%)
  1 (1.00%) low severe
  2 (2.00%) high mild
  3 (3.00%) high severe
linter/default-rules/numpy/ctypeslib.py
                        time:   [693.03 µs 693.43 µs 693.87 µs]
                        thrpt:  [23.997 MiB/s 24.013 MiB/s 24.027 MiB/s]
                 change:
                        time:   [-0.4169% -0.2614% -0.1292%] (p = 0.00 < 0.05)
                        thrpt:  [+0.1293% +0.2621% +0.4186%]
                        Change within noise threshold.
Found 6 outliers among 100 measurements (6.00%)
  4 (4.00%) low mild
  1 (1.00%) high mild
  1 (1.00%) high severe
linter/default-rules/large/dataset.py
                        time:   [3.3584 ms 3.3612 ms 3.3638 ms]
                        thrpt:  [12.094 MiB/s 12.104 MiB/s 12.114 MiB/s]
                 change:
                        time:   [-0.1874% -0.1139% -0.0460%] (p = 0.00 < 0.05)
                        thrpt:  [+0.0460% +0.1141% +0.1878%]
                        Change within noise threshold.
Found 6 outliers among 100 measurements (6.00%)
  4 (4.00%) low severe
  2 (2.00%) low mild

linter/all-rules/numpy/globals.py
                        time:   [148.26 µs 148.37 µs 148.47 µs]
                        thrpt:  [19.873 MiB/s 19.888 MiB/s 19.902 MiB/s]
                 change:
                        time:   [-2.0010% -1.5549% -1.0589%] (p = 0.00 < 0.05)
                        thrpt:  [+1.0703% +1.5794% +2.0419%]
                        Performance has improved.
Found 14 outliers among 100 measurements (14.00%)
  7 (7.00%) high mild
  7 (7.00%) high severe
linter/all-rules/pydantic/types.py
                        time:   [2.9326 ms 2.9349 ms 2.9376 ms]
                        thrpt:  [8.6817 MiB/s 8.6896 MiB/s 8.6965 MiB/s]
                 change:
                        time:   [-1.1966% -1.0700% -0.9392%] (p = 0.00 < 0.05)
                        thrpt:  [+0.9481% +1.0816% +1.2111%]
                        Change within noise threshold.
Found 5 outliers among 100 measurements (5.00%)
  4 (4.00%) high mild
  1 (1.00%) high severe
linter/all-rules/numpy/ctypeslib.py
                        time:   [1.6737 ms 1.6743 ms 1.6750 ms]
                        thrpt:  [9.9410 MiB/s 9.9450 MiB/s 9.9485 MiB/s]
                 change:
                        time:   [-1.4525% -1.2985% -1.1571%] (p = 0.00 < 0.05)
                        thrpt:  [+1.1707% +1.3156% +1.4739%]
                        Performance has improved.
Found 8 outliers among 100 measurements (8.00%)
  3 (3.00%) high mild
  5 (5.00%) high severe
linter/all-rules/large/dataset.py
                        time:   [6.9297 ms 6.9351 ms 6.9409 ms]
                        thrpt:  [5.8613 MiB/s 5.8663 MiB/s 5.8708 MiB/s]
                 change:
                        time:   [-1.4497% -1.3579% -1.2482%] (p = 0.00 < 0.05)
                        thrpt:  [+1.2640% +1.3766% +1.4711%]
                        Performance has improved.
Found 3 outliers among 100 measurements (3.00%)
  3 (3.00%) high mild
```

---

_Merged by @charliermarsh on 2023-06-04 02:28_

---

_Closed by @charliermarsh on 2023-06-04 02:28_

---

_Branch deleted on 2023-06-04 02:28_

---

_Comment by @github-actions[bot] on 2023-06-04 02:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.4±0.20ms     2.3 MB/sec    1.02     17.8±0.14ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.03ms     4.0 MB/sec    1.01      4.2±0.02ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    505.2±1.00µs     5.8 MB/sec    1.03    518.3±1.88µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.05ms     3.5 MB/sec    1.02      7.3±0.05ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.03ms     4.9 MB/sec    1.02      8.4±0.03ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1792.6±2.05µs     9.3 MB/sec    1.03   1838.1±4.86µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.5±0.43µs    14.5 MB/sec    1.03    208.9±1.15µs    14.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     6.8 MB/sec    1.01      3.8±0.01ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.4±0.01ms     6.3 MB/sec    1.00      6.4±0.02ms     6.4 MB/sec
parser/numpy/ctypeslib.py                  1.01   1257.4±2.64µs    13.2 MB/sec    1.00   1245.8±2.35µs    13.4 MB/sec
parser/numpy/globals.py                    1.01    130.1±0.22µs    22.7 MB/sec    1.00    128.9±3.28µs    22.9 MB/sec
parser/pydantic/types.py                   1.01      2.8±0.00ms     9.2 MB/sec    1.00      2.7±0.01ms     9.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.8±0.22ms     2.4 MB/sec    1.01     16.9±0.37ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.06ms     3.9 MB/sec    1.01      4.3±0.12ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    493.4±6.66µs     6.0 MB/sec    1.08   532.1±24.23µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.10ms     3.6 MB/sec    1.06      7.4±0.36ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.09ms     4.9 MB/sec    1.02      8.4±0.18ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1740.9±17.99µs     9.6 MB/sec    1.02  1777.7±23.38µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.7±5.58µs    14.5 MB/sec    1.00    204.0±6.57µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.07ms     6.8 MB/sec    1.01      3.8±0.05ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.4±0.04ms     6.3 MB/sec    1.00      6.4±0.07ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.01  1218.0±14.03µs    13.7 MB/sec    1.00  1205.4±20.65µs    13.8 MB/sec
parser/numpy/globals.py                    1.00    124.5±2.14µs    23.7 MB/sec    1.00    124.4±3.78µs    23.7 MB/sec
parser/pydantic/types.py                   1.01      2.7±0.03ms     9.3 MB/sec    1.00      2.7±0.04ms     9.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
