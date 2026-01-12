```yaml
number: 6667
title: Remove some unnecessary ampersands in the formatter
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/amper
created_at: 2023-08-18T04:07:33Z
updated_at: 2023-08-18T04:41:57Z
url: https://github.com/astral-sh/ruff/pull/6667
synced_at: 2026-01-12T02:52:04Z
```

# Remove some unnecessary ampersands in the formatter

---

_Pull request opened by @charliermarsh on 2023-08-18 04:07_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-08-18 04:14_

---

_Merged by @charliermarsh on 2023-08-18 04:18_

---

_Closed by @charliermarsh on 2023-08-18 04:18_

---

_Branch deleted on 2023-08-18 04:18_

---

_Comment by @github-actions[bot] on 2023-08-18 04:41_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.5±0.19ms    11.5 MB/sec    1.03      3.7±0.20ms    11.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   743.8±66.54µs    22.4 MB/sec    1.04   772.6±45.52µs    21.6 MB/sec
formatter/numpy/globals.py                 1.00     75.0±5.13µs    39.4 MB/sec    1.03     77.4±6.82µs    38.1 MB/sec
formatter/pydantic/types.py                1.00  1417.9±77.48µs    18.0 MB/sec    1.04  1470.1±72.51µs    17.3 MB/sec
linter/all-rules/large/dataset.py          1.01     12.2±0.30ms     3.3 MB/sec    1.00     12.1±0.28ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.10ms     5.1 MB/sec    1.01      3.3±0.10ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   451.3±15.62µs     6.5 MB/sec    1.07   481.2±19.46µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.21ms     4.1 MB/sec    1.03      6.4±0.19ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.01      6.6±0.26ms     6.1 MB/sec    1.00      6.6±0.15ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1426.8±32.97µs    11.7 MB/sec    1.00  1426.7±34.62µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    172.8±7.69µs    17.1 MB/sec    1.04    179.1±7.01µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.10ms     8.9 MB/sec    1.02      2.9±0.08ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      4.5±0.23ms     9.0 MB/sec     1.00      4.5±0.19ms     9.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   930.5±52.56µs    17.9 MB/sec     1.00   934.7±45.42µs    17.8 MB/sec
formatter/numpy/globals.py                 1.00     99.1±8.84µs    29.8 MB/sec     1.00     99.5±6.33µs    29.7 MB/sec
formatter/pydantic/types.py                1.00  1860.0±121.54µs    13.7 MB/sec    1.01  1886.9±72.08µs    13.5 MB/sec
linter/all-rules/large/dataset.py          1.02     15.7±0.32ms     2.6 MB/sec     1.00     15.4±0.27ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.12ms     3.9 MB/sec     1.00      4.2±0.13ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.03   537.6±22.64µs     5.5 MB/sec     1.00   520.6±18.45µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.1±0.18ms     3.1 MB/sec     1.00      7.9±0.20ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.01      8.6±0.16ms     4.7 MB/sec     1.00      8.5±0.17ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1833.1±53.58µs     9.1 MB/sec     1.00  1809.6±70.65µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.03    214.3±9.35µs    13.8 MB/sec     1.00    208.1±7.15µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.8±0.09ms     6.6 MB/sec     1.00      3.8±0.09ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
