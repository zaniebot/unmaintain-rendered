```yaml
number: 5087
title: "Remove `FixMode::None`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/fix-dry-run
created_at: 2023-06-14T15:06:05Z
updated_at: 2023-06-14T15:33:11Z
url: https://github.com/astral-sh/ruff/pull/5087
synced_at: 2026-01-12T15:55:17Z
```

# Remove `FixMode::None`

---

_@charliermarsh_

## Summary

We now _always_ generate fixes, so `FixMode::None` and `FixMode::Generate` are redundant. We can also remove the TODO around `--fix-dry-run`, since that's our default behavior.

Closes #5081.


---

_Merged by @charliermarsh on 2023-06-14 15:17_

---

_Closed by @charliermarsh on 2023-06-14 15:17_

---

_Branch deleted on 2023-06-14 15:17_

---

_Comment by @github-actions[bot] on 2023-06-14 15:17_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04      7.6±0.31ms     5.4 MB/sec    1.00      7.3±0.06ms     5.5 MB/sec
formatter/numpy/ctypeslib.py               1.07  1644.3±16.92µs    10.1 MB/sec    1.00  1539.2±14.71µs    10.8 MB/sec
formatter/numpy/globals.py                 1.06    157.7±1.83µs    18.7 MB/sec    1.00    148.6±1.99µs    19.9 MB/sec
formatter/pydantic/types.py                1.08      3.3±0.03ms     7.8 MB/sec    1.00      3.0±0.03ms     8.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.9±0.14ms     2.6 MB/sec    1.07     17.1±0.12ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.03ms     4.3 MB/sec    1.05      4.1±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    494.5±5.20µs     6.0 MB/sec    1.03    508.7±5.55µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.06ms     3.7 MB/sec    1.06      7.3±0.05ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.06ms     5.3 MB/sec    1.14      8.8±0.06ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1696.7±14.06µs     9.8 MB/sec    1.09  1856.8±16.72µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    185.3±4.44µs    15.9 MB/sec    1.09    201.6±2.06µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.10ms     7.2 MB/sec    1.12      3.9±0.03ms     6.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.5±0.39ms     4.3 MB/sec    1.04      9.9±0.26ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1914.8±57.23µs     8.7 MB/sec    1.05      2.0±0.07ms     8.3 MB/sec
formatter/numpy/globals.py                 1.00    190.0±6.81µs    15.5 MB/sec    1.12   213.4±17.13µs    13.8 MB/sec
formatter/pydantic/types.py                1.00      3.9±0.16ms     6.6 MB/sec    1.05      4.1±0.12ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.02     20.8±0.70ms  2004.6 KB/sec    1.00     20.4±0.61ms  2044.0 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.14ms     3.2 MB/sec    1.00      5.3±0.15ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.01   633.8±41.25µs     4.7 MB/sec    1.00   625.1±16.37µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.02      9.1±0.31ms     2.8 MB/sec    1.00      8.9±0.29ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.5±0.29ms     3.9 MB/sec    1.00     10.4±0.22ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.3±0.09ms     7.4 MB/sec    1.00      2.2±0.05ms     7.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    249.5±8.63µs    11.8 MB/sec    1.04    258.9±9.82µs    11.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.17ms     5.6 MB/sec    1.03      4.7±0.09ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
