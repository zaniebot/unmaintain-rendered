```yaml
number: 6882
title: Use the formatter prelude in more files
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/prelude
created_at: 2023-08-25T20:39:07Z
updated_at: 2023-08-25T21:05:24Z
url: https://github.com/astral-sh/ruff/pull/6882
synced_at: 2026-01-12T15:55:22Z
```

# Use the formatter prelude in more files

---

_@charliermarsh_

Removes a bunch of imports that are made redundant by the prelude.

---

_Label `formatter` added by @charliermarsh on 2023-08-25 20:39_

---

_Merged by @charliermarsh on 2023-08-25 20:51_

---

_Closed by @charliermarsh on 2023-08-25 20:51_

---

_Branch deleted on 2023-08-25 20:51_

---

_Comment by @github-actions[bot] on 2023-08-25 21:05_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.3±0.02ms     7.7 MB/sec    1.00      5.2±0.01ms     7.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   1038.4±3.93µs    16.0 MB/sec    1.00   1037.6±3.11µs    16.0 MB/sec
formatter/numpy/globals.py                 1.00     96.3±0.50µs    30.6 MB/sec    1.01     96.9±0.48µs    30.5 MB/sec
formatter/pydantic/types.py                1.00  1970.3±10.26µs    12.9 MB/sec    1.00   1962.8±9.60µs    13.0 MB/sec
linter/all-rules/large/dataset.py          1.06     12.8±0.03ms     3.2 MB/sec    1.00     12.0±0.06ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      3.3±0.03ms     5.0 MB/sec    1.00      3.2±0.02ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    458.8±2.09µs     6.4 MB/sec    1.00    452.8±0.63µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.04      6.5±0.02ms     3.9 MB/sec    1.00      6.3±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.12      7.1±0.03ms     5.7 MB/sec    1.00      6.4±0.02ms     6.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.07   1502.3±4.56µs    11.1 MB/sec    1.00   1398.2±6.19µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.04    170.9±4.05µs    17.3 MB/sec    1.00    163.6±3.59µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.08      3.1±0.01ms     8.2 MB/sec    1.00      2.9±0.02ms     8.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.3±0.08ms     7.7 MB/sec    1.00      5.3±0.06ms     7.7 MB/sec
formatter/numpy/ctypeslib.py               1.00  1021.2±21.64µs    16.3 MB/sec    1.01  1028.9±15.36µs    16.2 MB/sec
formatter/numpy/globals.py                 1.00     95.1±2.05µs    31.0 MB/sec    1.02     96.6±2.93µs    30.5 MB/sec
formatter/pydantic/types.py                1.00  1943.2±26.26µs    13.1 MB/sec    1.02  1982.5±55.14µs    12.9 MB/sec
linter/all-rules/large/dataset.py          1.02     12.9±0.19ms     3.2 MB/sec    1.00     12.7±0.16ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.05ms     4.8 MB/sec    1.00      3.4±0.04ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    433.5±6.44µs     6.8 MB/sec    1.00    432.8±7.12µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.7±0.13ms     3.8 MB/sec    1.00      6.6±0.10ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.08ms     5.8 MB/sec    1.00      7.0±0.10ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1495.0±25.53µs    11.1 MB/sec    1.00  1484.0±21.13µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.7±4.15µs    17.0 MB/sec    1.00    173.9±4.16µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.2±0.04ms     8.1 MB/sec    1.00      3.1±0.05ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
