```yaml
number: 5010
title: Consider submodule imports to detect unused imports
type: pull_request
state: closed
author: dhruvmanila
labels: []
assignees: []
draft: true
base: main
head: fix/unused-imports
created_at: 2023-06-10T18:59:44Z
updated_at: 2023-06-11T13:06:50Z
url: https://github.com/astral-sh/ruff/pull/5010
synced_at: 2026-01-12T15:55:17Z
```

# Consider submodule imports to detect unused imports

---

_@dhruvmanila_

## Summary

TODO

## Test Plan

TODO


---

_Comment by @github-actions[bot] on 2023-06-10 19:29_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.9±0.01ms     5.9 MB/sec    1.00      6.8±0.03ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.01  1393.1±22.63µs    12.0 MB/sec    1.00   1384.6±1.98µs    12.0 MB/sec
formatter/numpy/globals.py                 1.00    137.8±0.50µs    21.4 MB/sec    1.00    137.6±0.59µs    21.4 MB/sec
formatter/pydantic/types.py                1.01      2.8±0.01ms     9.1 MB/sec    1.00      2.8±0.01ms     9.2 MB/sec
linter/all-rules/large/dataset.py          1.01     14.8±0.07ms     2.7 MB/sec    1.00     14.6±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.7 MB/sec    1.00      3.6±0.00ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    366.1±1.37µs     8.1 MB/sec    1.02    371.6±0.95µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.02ms     4.1 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.02ms     5.5 MB/sec    1.01      7.4±0.02ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1534.6±4.13µs    10.9 MB/sec    1.01   1550.5±3.72µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.7±0.28µs    17.9 MB/sec    1.02    167.4±0.22µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.01      3.3±0.01ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.7±0.06ms     5.3 MB/sec    1.08      8.3±0.08ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1563.3±15.15µs    10.7 MB/sec    1.07  1671.3±22.36µs    10.0 MB/sec
formatter/numpy/globals.py                 1.00    151.3±3.70µs    19.5 MB/sec    1.05    158.5±4.37µs    18.6 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.08ms     8.1 MB/sec    1.09      3.4±0.05ms     7.5 MB/sec
linter/all-rules/large/dataset.py          1.01     16.5±0.60ms     2.5 MB/sec    1.00     16.4±0.14ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.07ms     4.0 MB/sec    1.02      4.2±0.12ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    494.1±7.38µs     6.0 MB/sec    1.02    504.4±8.97µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.06ms     3.7 MB/sec    1.01      7.0±0.07ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.07ms     4.9 MB/sec    1.00      8.3±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1750.8±16.64µs     9.5 MB/sec    1.01  1765.8±18.13µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.8±3.17µs    14.7 MB/sec    1.03    207.8±4.19µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.03ms     6.8 MB/sec    1.00      3.7±0.04ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @dhruvmanila on 2023-06-11 02:28_

closing in favor of #5011 

---

_Closed by @dhruvmanila on 2023-06-11 02:28_

---

_Branch deleted on 2023-06-11 02:28_

---
