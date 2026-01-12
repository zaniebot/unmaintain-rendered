```yaml
number: 6404
title: "Rename `ArgumentSeparator` to `ParameterSeparator`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/parameters
created_at: 2023-08-07T19:34:49Z
updated_at: 2023-08-07T20:01:53Z
url: https://github.com/astral-sh/ruff/pull/6404
synced_at: 2026-01-12T02:52:04Z
```

# Rename `ArgumentSeparator` to `ParameterSeparator`

---

_Pull request opened by @charliermarsh on 2023-08-07 19:34_

To mirror the rename from `Arguments` to `Parameters`.

---

_Label `formatter` added by @charliermarsh on 2023-08-07 19:34_

---

_@zanieb approved on 2023-08-07 19:45_

---

_Merged by @charliermarsh on 2023-08-07 19:46_

---

_Closed by @charliermarsh on 2023-08-07 19:46_

---

_Branch deleted on 2023-08-07 19:46_

---

_Comment by @github-actions[bot] on 2023-08-07 20:01_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.5±0.38ms     3.9 MB/sec    1.01     10.6±0.39ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.1±0.11ms     8.0 MB/sec    1.00      2.1±0.11ms     8.1 MB/sec
formatter/numpy/globals.py                 1.00   229.2±11.73µs    12.9 MB/sec    1.03   235.4±12.48µs    12.5 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.18ms     5.8 MB/sec    1.03      4.5±0.21ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.00     14.4±0.37ms     2.8 MB/sec    1.01     14.5±0.36ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.8±0.09ms     4.4 MB/sec    1.00      3.7±0.11ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   529.5±28.03µs     5.6 MB/sec    1.00   527.1±17.77µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.21ms     3.9 MB/sec    1.00      6.6±0.18ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.01      8.0±0.22ms     5.1 MB/sec    1.00      7.9±0.24ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1677.1±66.90µs     9.9 MB/sec    1.00  1646.1±49.39µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.02    196.6±7.04µs    15.0 MB/sec    1.00   192.9±10.75µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.6±0.20ms     7.1 MB/sec    1.00      3.5±0.11ms     7.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.10ms     5.2 MB/sec    1.00      7.8±0.06ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1488.3±18.92µs    11.2 MB/sec    1.00   1486.9±9.59µs    11.2 MB/sec
formatter/numpy/globals.py                 1.02    153.9±2.31µs    19.2 MB/sec    1.00    150.5±1.05µs    19.6 MB/sec
formatter/pydantic/types.py                1.01      3.3±0.02ms     7.7 MB/sec    1.00      3.3±0.02ms     7.8 MB/sec
linter/all-rules/large/dataset.py          1.00     10.1±0.05ms     4.0 MB/sec    1.00     10.1±0.08ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.02ms     6.1 MB/sec    1.00      2.7±0.01ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    291.4±2.30µs    10.1 MB/sec    1.00    290.4±2.31µs    10.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.6±0.02ms     5.6 MB/sec    1.00      4.6±0.01ms     5.6 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.04ms     7.5 MB/sec    1.00      5.4±0.03ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1085.2±5.93µs    15.3 MB/sec    1.00   1077.0±7.95µs    15.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    113.3±1.29µs    26.0 MB/sec    1.00    112.9±1.03µs    26.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.4±0.01ms    10.8 MB/sec    1.00      2.3±0.01ms    10.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
