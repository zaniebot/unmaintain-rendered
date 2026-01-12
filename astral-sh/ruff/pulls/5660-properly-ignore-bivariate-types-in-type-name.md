```yaml
number: 5660
title: "Properly ignore bivariate types in `type-name-incorrect-variance`"
type: pull_request
state: merged
author: tjkuson
labels: []
assignees: []
merged: true
base: main
head: bivariance
created_at: 2023-07-10T17:55:22Z
updated_at: 2023-07-16T11:52:12Z
url: https://github.com/astral-sh/ruff/pull/5660
synced_at: 2026-01-12T03:30:21Z
```

# Properly ignore bivariate types in `type-name-incorrect-variance`

---

_Pull request opened by @tjkuson on 2023-07-10 17:55_

## Summary

#5658 didn't actually ignore bivariate types in some all cases (sorry about that). This PR fixes that and adds bivariate types to the test fixture.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-07-10 18:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.4±0.01ms     4.8 MB/sec    1.00      8.4±0.03ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.01   1805.2±3.47µs     9.2 MB/sec    1.00   1791.6±4.94µs     9.3 MB/sec
formatter/numpy/globals.py                 1.00    197.8±2.05µs    14.9 MB/sec    1.00    198.5±1.19µs    14.9 MB/sec
formatter/pydantic/types.py                1.02      4.1±0.01ms     6.2 MB/sec    1.00      4.0±0.00ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     13.9±0.06ms     2.9 MB/sec    1.01     14.1±0.03ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.8 MB/sec    1.01      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    361.2±1.03µs     8.2 MB/sec    1.02    369.2±1.33µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.01      6.3±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.01ms     5.7 MB/sec    1.02      7.3±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1454.5±1.97µs    11.4 MB/sec    1.03   1493.4±3.95µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    156.1±0.81µs    18.9 MB/sec    1.03    160.9±0.33µs    18.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.03ms     8.1 MB/sec    1.02      3.2±0.01ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03     11.9±0.29ms     3.4 MB/sec    1.00     11.6±0.27ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.6±0.09ms     6.5 MB/sec    1.00      2.5±0.09ms     6.6 MB/sec
formatter/numpy/globals.py                 1.02   299.8±22.62µs     9.8 MB/sec    1.00   292.5±10.04µs    10.1 MB/sec
formatter/pydantic/types.py                1.01      5.7±0.16ms     4.5 MB/sec    1.00      5.6±0.20ms     4.5 MB/sec
linter/all-rules/large/dataset.py          1.00     19.3±0.40ms     2.1 MB/sec    1.19     23.0±0.78ms  1811.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.19ms     3.2 MB/sec    1.02      5.3±0.24ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   639.6±26.59µs     4.6 MB/sec    1.06   680.6±62.92µs     4.3 MB/sec
linter/all-rules/pydantic/types.py         1.03      9.4±0.55ms     2.7 MB/sec    1.00      9.1±0.33ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.07     10.7±0.44ms     3.8 MB/sec    1.00     10.0±0.37ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04      2.2±0.07ms     7.5 MB/sec    1.00      2.1±0.07ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.03    262.6±9.19µs    11.2 MB/sec    1.00    254.8±7.30µs    11.6 MB/sec
linter/default-rules/pydantic/types.py     1.03      4.6±0.17ms     5.5 MB/sec    1.00      4.5±0.17ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-10 18:19_

---

_Closed by @charliermarsh on 2023-07-10 18:19_

---

_Branch deleted on 2023-07-16 11:52_

---
