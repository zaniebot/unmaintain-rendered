```yaml
number: 6188
title: "CI Test: Debuggable formatter ecosystem ci failure"
type: pull_request
state: closed
author: konstin
labels: []
assignees: []
draft: true
base: main
head: debuggable-formatter-ecosystem-ci-failure
created_at: 2023-07-31T09:38:14Z
updated_at: 2023-08-24T12:39:17Z
url: https://github.com/astral-sh/ruff/pull/6188
synced_at: 2026-01-12T15:55:20Z
```

# CI Test: Debuggable formatter ecosystem ci failure

---

_@konstin_

(CI test)

---

_Comment by @github-actions[bot] on 2023-07-31 10:11_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.5±0.42ms     3.5 MB/sec    1.02     11.7±0.47ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.14ms     7.5 MB/sec    1.03      2.3±0.15ms     7.2 MB/sec
formatter/numpy/globals.py                 1.00   247.8±15.66µs    11.9 MB/sec    1.02   252.8±22.98µs    11.7 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.16ms     5.3 MB/sec    1.04      5.0±0.26ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.00     16.0±0.48ms     2.5 MB/sec    1.04     16.6±0.55ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.18ms     4.3 MB/sec    1.01      3.9±0.14ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.04   548.6±46.67µs     5.4 MB/sec    1.00   526.4±19.56µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.2±0.36ms     3.6 MB/sec    1.00      7.1±0.17ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.26ms     4.8 MB/sec    1.06      9.0±0.48ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1706.1±54.50µs     9.8 MB/sec    1.04  1771.0±45.41µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    193.6±9.03µs    15.2 MB/sec    1.04    201.7±6.99µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.09ms     7.0 MB/sec    1.05      3.8±0.12ms     6.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.2±0.06ms     4.0 MB/sec    1.00     10.2±0.13ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.01  1954.4±13.50µs     8.5 MB/sec    1.00  1934.0±14.40µs     8.6 MB/sec
formatter/numpy/globals.py                 1.01    202.9±2.46µs    14.5 MB/sec    1.00    200.6±4.75µs    14.7 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.03ms     5.9 MB/sec    1.00      4.3±0.05ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.05ms     3.0 MB/sec    1.00     13.6±0.06ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.6 MB/sec    1.00      3.6±0.03ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    366.4±4.60µs     8.1 MB/sec    1.00    364.7±6.81µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.04ms     4.1 MB/sec    1.00      6.2±0.04ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.7±0.03ms     5.3 MB/sec    1.00      7.7±0.02ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1526.4±9.92µs    10.9 MB/sec    1.00  1505.3±10.29µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    151.6±1.12µs    19.5 MB/sec    1.00    150.2±2.60µs    19.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.4±0.02ms     7.6 MB/sec    1.00      3.3±0.01ms     7.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Closed by @konstin on 2023-07-31 12:45_

---

_Branch deleted on 2023-08-24 12:39_

---
