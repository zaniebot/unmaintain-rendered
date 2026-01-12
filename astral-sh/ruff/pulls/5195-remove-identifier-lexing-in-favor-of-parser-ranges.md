```yaml
number: 5195
title: Remove identifier lexing in favor of parser ranges
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/lex
created_at: 2023-06-19T22:29:15Z
updated_at: 2023-06-20T16:41:26Z
url: https://github.com/astral-sh/ruff/pull/5195
synced_at: 2026-01-12T15:55:17Z
```

# Remove identifier lexing in favor of parser ranges

---

_@charliermarsh_

## Summary

Now that all identifiers include ranges (#5194), we can remove a ton of this "custom lexing" code that we have to sketchily extract identifier ranges from source.

## Test Plan

`cargo test`


---

_Comment by @charliermarsh on 2023-06-19 22:33_

I'm going to benchmark this manually.

---

_Comment by @github-actions[bot] on 2023-06-19 22:47_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.6±0.23ms     4.7 MB/sec    1.00      8.5±0.22ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1795.3±60.05µs     9.3 MB/sec    1.02  1833.9±62.08µs     9.1 MB/sec
formatter/numpy/globals.py                 1.00   181.0±12.47µs    16.3 MB/sec    1.00    181.8±9.11µs    16.2 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.12ms     7.2 MB/sec    1.03      3.6±0.14ms     7.0 MB/sec
linter/all-rules/large/dataset.py          1.03     18.8±0.62ms     2.2 MB/sec    1.00     18.2±0.46ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.13ms     3.8 MB/sec    1.02      4.4±0.18ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   573.0±16.04µs     5.1 MB/sec    1.06   605.1±47.61µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.8±0.18ms     3.3 MB/sec    1.02      8.0±0.23ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.8±0.13ms     4.6 MB/sec    1.05      9.2±0.54ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1980.5±95.25µs     8.4 MB/sec    1.00  1905.4±50.89µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   231.2±12.03µs    12.8 MB/sec    1.00   232.3±12.35µs    12.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.14ms     6.2 MB/sec    1.01      4.2±0.22ms     6.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.4±0.10ms     5.5 MB/sec    1.02      7.6±0.10ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.00  1529.0±36.20µs    10.9 MB/sec    1.03  1577.0±28.59µs    10.6 MB/sec
formatter/numpy/globals.py                 1.00    151.2±2.82µs    19.5 MB/sec    1.05    159.0±4.81µs    18.6 MB/sec
formatter/pydantic/types.py                1.00      3.0±0.04ms     8.4 MB/sec    1.03      3.1±0.06ms     8.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.9±0.11ms     2.7 MB/sec    1.00     14.9±0.16ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.9±0.04ms     4.2 MB/sec    1.00      3.9±0.03ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.01    493.7±6.16µs     6.0 MB/sec    1.00    488.5±5.71µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.05ms     3.8 MB/sec    1.00      6.7±0.06ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.03ms     5.2 MB/sec    1.00      7.7±0.04ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1681.7±36.68µs     9.9 MB/sec    1.00  1664.8±27.84µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.02    194.6±4.40µs    15.2 MB/sec    1.00    191.0±3.77µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.5±0.03ms     7.2 MB/sec    1.00      3.5±0.03ms     7.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @charliermarsh on 2023-06-19 23:28_

---

_Review requested from @konstin by @charliermarsh on 2023-06-19 23:28_

---

_Comment by @charliermarsh on 2023-06-19 23:28_

I benchmarked locally -- no significant change:

```rust
linter/default-rules/numpy/globals.py
                        time:   [70.940 µs 70.950 µs 70.960 µs]
                        thrpt:  [41.582 MiB/s 41.588 MiB/s 41.594 MiB/s]
                 change:
                        time:   [+0.1182% +0.2529% +0.3790%] (p = 0.00 < 0.05)
                        thrpt:  [-0.3776% -0.2523% -0.1181%]
                        Change within noise threshold.
Found 7 outliers among 100 measurements (7.00%)
  5 (5.00%) high mild
  2 (2.00%) high severe
linter/default-rules/pydantic/types.py
                        time:   [1.4309 ms 1.4317 ms 1.4326 ms]
                        thrpt:  [17.802 MiB/s 17.813 MiB/s 17.824 MiB/s]
                 change:
                        time:   [-0.1750% +0.1453% +0.4027%] (p = 0.36 > 0.05)
                        thrpt:  [-0.4011% -0.1451% +0.1753%]
                        No change in performance detected.
Found 1 outliers among 100 measurements (1.00%)
  1 (1.00%) high severe
linter/default-rules/numpy/ctypeslib.py
                        time:   [662.08 µs 663.16 µs 664.66 µs]
                        thrpt:  [25.052 MiB/s 25.109 MiB/s 25.150 MiB/s]
                 change:
                        time:   [+0.0799% +0.3360% +0.6133%] (p = 0.02 < 0.05)
                        thrpt:  [-0.6095% -0.3349% -0.0799%]
                        Change within noise threshold.
linter/default-rules/large/dataset.py
                        time:   [3.2802 ms 3.2829 ms 3.2862 ms]
                        thrpt:  [12.380 MiB/s 12.392 MiB/s 12.403 MiB/s]
                 change:
                        time:   [+0.9165% +1.0560% +1.1893%] (p = 0.00 < 0.05)
                        thrpt:  [-1.1753% -1.0450% -0.9082%]
                        Change within noise threshold.
Found 15 outliers among 100 measurements (15.00%)
  3 (3.00%) high mild
  12 (12.00%) high severe

linter/all-rules/numpy/globals.py
                        time:   [139.61 µs 139.64 µs 139.67 µs]
                        thrpt:  [21.126 MiB/s 21.131 MiB/s 21.135 MiB/s]
                 change:
                        time:   [-0.3035% -0.1199% +0.0434%] (p = 0.18 > 0.05)
                        thrpt:  [-0.0434% +0.1200% +0.3044%]
                        No change in performance detected.
Found 1 outliers among 100 measurements (1.00%)
  1 (1.00%) high severe
linter/all-rules/pydantic/types.py
                        time:   [2.7494 ms 2.7519 ms 2.7555 ms]
                        thrpt:  [9.2552 MiB/s 9.2674 MiB/s 9.2758 MiB/s]
                 change:
                        time:   [-0.4897% -0.2531% -0.0332%] (p = 0.03 < 0.05)
                        thrpt:  [+0.0332% +0.2537% +0.4921%]
                        Change within noise threshold.
Found 9 outliers among 100 measurements (9.00%)
  3 (3.00%) high mild
  6 (6.00%) high severe
linter/all-rules/numpy/ctypeslib.py
                        time:   [1.5430 ms 1.5435 ms 1.5441 ms]
                        thrpt:  [10.784 MiB/s 10.788 MiB/s 10.791 MiB/s]
                 change:
                        time:   [+0.4193% +0.5350% +0.6285%] (p = 0.00 < 0.05)
                        thrpt:  [-0.6246% -0.5322% -0.4175%]
                        Change within noise threshold.
Found 10 outliers among 100 measurements (10.00%)
  6 (6.00%) high mild
  4 (4.00%) high severe
linter/all-rules/large/dataset.py
                        time:   [6.4215 ms 6.4227 ms 6.4241 ms]
                        thrpt:  [6.3329 MiB/s 6.3342 MiB/s 6.3354 MiB/s]
                 change:
                        time:   [+0.1421% +0.2731% +0.3995%] (p = 0.00 < 0.05)
                        thrpt:  [-0.3979% -0.2724% -0.1419%]
                        Change within noise threshold.
Found 9 outliers among 100 measurements (9.00%)
  4 (4.00%) high mild
  5 (5.00%) high severe
```

---

_Comment by @charliermarsh on 2023-06-19 23:28_

Which is good, because we were worried that the parser might get slower.

---

_@MichaReiser approved on 2023-06-20 05:21_

---

_Label `internal` added by @MichaReiser on 2023-06-20 16:07_

---

_Merged by @charliermarsh on 2023-06-20 16:07_

---

_Closed by @charliermarsh on 2023-06-20 16:07_

---

_Branch deleted on 2023-06-20 16:07_

---
