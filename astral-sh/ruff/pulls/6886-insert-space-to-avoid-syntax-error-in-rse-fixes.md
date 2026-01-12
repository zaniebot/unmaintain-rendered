```yaml
number: 6886
title: Insert space to avoid syntax error in RSE fixes
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - fuzzer
assignees: []
merged: true
base: main
head: charlie/RSE
created_at: 2023-08-25T22:18:47Z
updated_at: 2023-08-25T22:57:49Z
url: https://github.com/astral-sh/ruff/pull/6886
synced_at: 2026-01-12T02:45:38Z
```

# Insert space to avoid syntax error in RSE fixes

---

_Pull request opened by @charliermarsh on 2023-08-25 22:18_

Closes https://github.com/astral-sh/ruff/issues/6810.

---

_Label `bug` added by @charliermarsh on 2023-08-25 22:18_

---

_Label `fuzzer` added by @charliermarsh on 2023-08-25 22:18_

---

_Merged by @charliermarsh on 2023-08-25 22:26_

---

_Closed by @charliermarsh on 2023-08-25 22:26_

---

_Branch deleted on 2023-08-25 22:26_

---

_Comment by @github-actions[bot] on 2023-08-25 22:29_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.2±0.05ms     7.8 MB/sec    1.00      5.2±0.04ms     7.8 MB/sec
formatter/numpy/ctypeslib.py               1.01  1036.6±11.45µs    16.1 MB/sec    1.00  1030.6±12.69µs    16.2 MB/sec
formatter/numpy/globals.py                 1.01     98.5±1.26µs    30.0 MB/sec    1.00     97.8±1.64µs    30.2 MB/sec
formatter/pydantic/types.py                1.01  1972.6±23.26µs    12.9 MB/sec    1.00  1962.2±21.38µs    13.0 MB/sec
linter/all-rules/large/dataset.py          1.02     11.9±0.12ms     3.4 MB/sec    1.00     11.7±0.09ms     3.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.2±0.02ms     5.2 MB/sec    1.00      3.2±0.04ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    446.9±5.40µs     6.6 MB/sec    1.00    446.2±3.76µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.04ms     4.1 MB/sec    1.00      6.1±0.05ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.3±0.05ms     6.5 MB/sec    1.00      6.2±0.04ms     6.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1394.0±13.15µs    11.9 MB/sec    1.00  1388.5±10.98µs    12.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    161.5±1.83µs    18.3 MB/sec    1.00    159.4±2.09µs    18.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.03ms     9.0 MB/sec    1.01      2.9±0.02ms     8.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.1±0.25ms     6.7 MB/sec    1.02      6.2±0.22ms     6.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1167.9±49.36µs    14.3 MB/sec    1.05  1220.7±45.86µs    13.6 MB/sec
formatter/numpy/globals.py                 1.00    112.1±6.18µs    26.3 MB/sec    1.02   114.0±11.06µs    25.9 MB/sec
formatter/pydantic/types.py                1.00      2.3±0.10ms    11.2 MB/sec    1.01      2.3±0.10ms    11.0 MB/sec
linter/all-rules/large/dataset.py          1.00     14.8±0.34ms     2.7 MB/sec    1.01     14.9±0.54ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.15ms     4.1 MB/sec    1.02      4.1±0.17ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01   521.7±26.36µs     5.7 MB/sec    1.00   515.8±28.22µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.7±0.23ms     3.3 MB/sec    1.02      7.9±0.28ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.22ms     5.0 MB/sec    1.04      8.5±0.30ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1763.6±76.59µs     9.4 MB/sec    1.04  1826.6±85.68µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00   207.6±10.86µs    14.2 MB/sec    1.02   211.4±12.73µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.12ms     6.8 MB/sec    1.03      3.8±0.15ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
