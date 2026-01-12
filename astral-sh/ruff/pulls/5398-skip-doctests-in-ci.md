```yaml
number: 5398
title: Skip doctests in CI
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/doc-tests
created_at: 2023-06-27T16:52:53Z
updated_at: 2023-06-27T17:22:57Z
url: https://github.com/astral-sh/ruff/pull/5398
synced_at: 2026-01-12T03:36:55Z
```

# Skip doctests in CI

---

_Pull request opened by @charliermarsh on 2023-06-27 16:52_

(This is just for benchmarking.)

---

_Comment by @charliermarsh on 2023-06-27 16:57_

Looks like the test step itself goes down to 1m35s (Unix) and 1m45s (Windows).

Before, I see 2m36s (Unix) and 3m21s (Windows) on an arbitrarily selected PR.


---

_Closed by @charliermarsh on 2023-06-27 16:57_

---

_Branch deleted on 2023-06-27 16:57_

---

_Comment by @github-actions[bot] on 2023-06-27 17:22_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.2±0.04ms     4.9 MB/sec    1.00      8.2±0.02ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   1713.7±1.91µs     9.7 MB/sec    1.00   1710.3±4.23µs     9.7 MB/sec
formatter/numpy/globals.py                 1.00    188.0±0.77µs    15.7 MB/sec    1.00    187.6±0.46µs    15.7 MB/sec
formatter/pydantic/types.py                1.01      3.9±0.03ms     6.6 MB/sec    1.00      3.8±0.01ms     6.6 MB/sec
linter/all-rules/large/dataset.py          1.00     13.5±0.02ms     3.0 MB/sec    1.01     13.6±0.02ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.8 MB/sec    1.00      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    361.2±1.15µs     8.2 MB/sec    1.00    362.0±1.32µs     8.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.01ms     4.2 MB/sec    1.00      6.0±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.01ms     5.8 MB/sec    1.00      7.0±0.01ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1472.2±6.12µs    11.3 MB/sec    1.00   1468.0±2.45µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    157.2±0.29µs    18.8 MB/sec    1.00    156.8±0.66µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.00ms     8.0 MB/sec    1.00      3.2±0.00ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.5±0.18ms     4.3 MB/sec    1.02      9.7±0.13ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1946.5±16.75µs     8.6 MB/sec    1.03      2.0±0.04ms     8.3 MB/sec
formatter/numpy/globals.py                 1.00    211.8±2.75µs    13.9 MB/sec    1.02    215.5±7.13µs    13.7 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.05ms     5.7 MB/sec    1.01      4.5±0.06ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.00     15.9±0.12ms     2.6 MB/sec    1.00     16.0±0.26ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.08ms     3.9 MB/sec    1.00      4.2±0.08ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01   441.4±10.37µs     6.7 MB/sec    1.00    438.9±7.00µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.19ms     3.5 MB/sec    1.00      7.2±0.19ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.09ms     5.0 MB/sec    1.00      8.2±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1723.7±17.52µs     9.7 MB/sec    1.00  1716.4±12.02µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.01    182.8±1.98µs    16.1 MB/sec    1.00    181.8±2.56µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.08ms     6.8 MB/sec    1.01      3.8±0.07ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
