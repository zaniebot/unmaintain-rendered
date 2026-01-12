```yaml
number: 5099
title: "Use `matches!` for `CallPath` comparisons"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/call-path
created_at: 2023-06-14T19:33:51Z
updated_at: 2023-06-14T21:06:36Z
url: https://github.com/astral-sh/ruff/pull/5099
synced_at: 2026-01-12T15:55:17Z
```

# Use `matches!` for `CallPath` comparisons

---

_@charliermarsh_

## Summary

This PR consistently uses `matches! for static `CallPath` comparisons. In some cases, we can significantly reduce the number of cases or checks.

## Test Plan

`cargo test `


---

_Label `internal` added by @charliermarsh on 2023-06-14 19:33_

---

_@konstin approved on 2023-06-14 19:39_

Do we know whether or rather when this makes a difference for the code generation? i find it hard to judge where we need manual optimizations such as these and when llvm will just figure out the right thing (apart from looking at the benchmarks, of course)

---

_Comment by @github-actions[bot] on 2023-06-14 19:41_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.4±0.04ms     6.4 MB/sec    1.01      6.4±0.03ms     6.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1315.4±3.94µs    12.7 MB/sec    1.00   1311.0±3.40µs    12.7 MB/sec
formatter/numpy/globals.py                 1.00    125.2±0.66µs    23.6 MB/sec    1.00    125.1±0.16µs    23.6 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.02ms     9.9 MB/sec    1.00      2.6±0.01ms     9.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.5±0.08ms     3.0 MB/sec    1.02     13.7±0.17ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.01      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    418.6±0.87µs     7.0 MB/sec    1.00    419.4±0.78µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.02ms     4.4 MB/sec    1.01      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.03ms     6.2 MB/sec    1.05      6.9±0.05ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1432.9±1.38µs    11.6 MB/sec    1.04   1491.3±4.95µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.2±0.25µs    18.2 MB/sec    1.04    168.3±0.70µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.02ms     8.4 MB/sec    1.03      3.1±0.03ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.7±0.11ms     5.3 MB/sec    1.00      7.6±0.08ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.01  1581.4±22.56µs    10.5 MB/sec    1.00  1569.4±102.07µs    10.6 MB/sec
formatter/numpy/globals.py                 1.03    151.0±8.58µs    19.5 MB/sec    1.00    147.0±4.40µs    20.1 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.04ms     8.1 MB/sec    1.03      3.3±0.22ms     7.8 MB/sec
linter/all-rules/large/dataset.py          1.01     16.6±0.21ms     2.5 MB/sec    1.00     16.3±0.21ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.2±0.06ms     4.0 MB/sec    1.00      4.1±0.06ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.05    498.3±8.26µs     5.9 MB/sec    1.00    475.1±6.20µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.2±0.09ms     3.5 MB/sec    1.00      7.0±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.10ms     4.9 MB/sec    1.00      8.3±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1753.4±43.14µs     9.5 MB/sec    1.00  1723.3±33.12µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.03    196.7±4.60µs    15.0 MB/sec    1.00    190.7±3.89µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.03ms     6.8 MB/sec    1.00      3.7±0.06ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-06-14 19:41_

I honestly don't have great intuition on that. I _have_ to assume that it's more performant in a case like this, where `matches!` can presumedly short-circuit if the module name (first element) doesn't match:

<img width="579" alt="Screen Shot 2023-06-14 at 3 41 07 PM" src="https://github.com/astral-sh/ruff/assets/1309177/814c6620-d251-424e-bc02-d23feb2fa667">


---

_Comment by @charliermarsh on 2023-06-14 20:09_

I'll try to run the benchmarks locally.

---

_Comment by @charliermarsh on 2023-06-14 21:06_

Wow, this actually had a measurable improvement for all-rules:

```
linter/default-rules/numpy/globals.py
                        time:   [73.665 µs 73.866 µs 74.066 µs]
                        thrpt:  [39.838 MiB/s 39.946 MiB/s 40.055 MiB/s]
                 change:
                        time:   [-1.4205% -0.9919% -0.5879%] (p = 0.00 < 0.05)
                        thrpt:  [+0.5914% +1.0019% +1.4410%]
                        Change within noise threshold.
Found 1 outliers among 100 measurements (1.00%)
  1 (1.00%) high mild
linter/default-rules/pydantic/types.py
                        time:   [1.4733 ms 1.4763 ms 1.4795 ms]
                        thrpt:  [17.238 MiB/s 17.275 MiB/s 17.311 MiB/s]
                 change:
                        time:   [-0.0405% +0.1718% +0.3813%] (p = 0.11 > 0.05)
                        thrpt:  [-0.3798% -0.1715% +0.0405%]
                        No change in performance detected.
Found 3 outliers among 100 measurements (3.00%)
  3 (3.00%) high mild
linter/default-rules/numpy/ctypeslib.py
                        time:   [675.29 µs 676.37 µs 677.38 µs]
                        thrpt:  [24.582 MiB/s 24.618 MiB/s 24.658 MiB/s]
                 change:
                        time:   [-0.6137% -0.3418% -0.0642%] (p = 0.01 < 0.05)
                        thrpt:  [+0.0643% +0.3430% +0.6175%]
                        Change within noise threshold.
Found 1 outliers among 100 measurements (1.00%)
  1 (1.00%) high mild
linter/default-rules/large/dataset.py
                        time:   [3.3271 ms 3.3328 ms 3.3382 ms]
                        thrpt:  [12.187 MiB/s 12.207 MiB/s 12.228 MiB/s]
                 change:
                        time:   [-0.8751% -0.6511% -0.4378%] (p = 0.00 < 0.05)
                        thrpt:  [+0.4397% +0.6553% +0.8828%]
                        Change within noise threshold.
Found 15 outliers among 100 measurements (15.00%)
  3 (3.00%) low severe
  10 (10.00%) low mild
  2 (2.00%) high mild

linter/all-rules/numpy/globals.py
                        time:   [147.07 µs 147.18 µs 147.29 µs]
                        thrpt:  [20.034 MiB/s 20.048 MiB/s 20.062 MiB/s]
                 change:
                        time:   [-4.1399% -3.4025% -2.7085%] (p = 0.00 < 0.05)
                        thrpt:  [+2.7839% +3.5223% +4.3187%]
                        Performance has improved.
Found 4 outliers among 100 measurements (4.00%)
  3 (3.00%) high mild
  1 (1.00%) high severe
linter/all-rules/pydantic/types.py
                        time:   [2.9727 ms 2.9746 ms 2.9766 ms]
                        thrpt:  [8.5679 MiB/s 8.5738 MiB/s 8.5790 MiB/s]
                 change:
                        time:   [-1.6400% -1.4298% -1.2218%] (p = 0.00 < 0.05)
                        thrpt:  [+1.2369% +1.4506% +1.6673%]
                        Performance has improved.
Found 12 outliers among 100 measurements (12.00%)
  12 (12.00%) high mild
linter/all-rules/numpy/ctypeslib.py
                        time:   [1.6569 ms 1.6586 ms 1.6606 ms]
                        thrpt:  [10.027 MiB/s 10.039 MiB/s 10.050 MiB/s]
                 change:
                        time:   [-2.6992% -2.3425% -2.0062%] (p = 0.00 < 0.05)
                        thrpt:  [+2.0473% +2.3987% +2.7741%]
                        Performance has improved.
Found 13 outliers among 100 measurements (13.00%)
  4 (4.00%) high mild
  9 (9.00%) high severe
linter/all-rules/large/dataset.py
                        time:   [7.0487 ms 7.0601 ms 7.0717 ms]
                        thrpt:  [5.7529 MiB/s 5.7624 MiB/s 5.7717 MiB/s]
                 change:
                        time:   [-2.4355% -2.2669% -2.1152%] (p = 0.00 < 0.05)
                        thrpt:  [+2.1609% +2.3195% +2.4963%]
                        Performance has improved.
Found 1 outliers among 100 measurements (1.00%)
  1 (1.00%) high mild
```

Note the last four entries -- all statistically significant improvements of 1.4% to 3.4%.

---

_Merged by @charliermarsh on 2023-06-14 21:06_

---

_Closed by @charliermarsh on 2023-06-14 21:06_

---

_Branch deleted on 2023-06-14 21:06_

---
