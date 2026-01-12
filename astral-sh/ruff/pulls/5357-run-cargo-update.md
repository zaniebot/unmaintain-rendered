```yaml
number: 5357
title: "Run `cargo update`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/lock
created_at: 2023-06-25T21:52:56Z
updated_at: 2023-06-25T22:28:13Z
url: https://github.com/astral-sh/ruff/pull/5357
synced_at: 2026-01-12T03:36:55Z
```

# Run `cargo update`

---

_Pull request opened by @charliermarsh on 2023-06-25 21:52_

_No description provided._

---

_Comment by @github-actions[bot] on 2023-06-25 22:06_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.7±0.05ms     6.1 MB/sec    1.00      6.6±0.02ms     6.1 MB/sec
formatter/numpy/ctypeslib.py               1.01   1562.0±1.57µs    10.7 MB/sec    1.00   1547.2±3.91µs    10.8 MB/sec
formatter/numpy/globals.py                 1.03    191.4±2.36µs    15.4 MB/sec    1.00    185.4±0.25µs    15.9 MB/sec
formatter/pydantic/types.py                1.01      3.5±0.01ms     7.3 MB/sec    1.00      3.4±0.01ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.1±0.03ms     3.1 MB/sec    1.02     13.3±0.12ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.00ms     5.0 MB/sec    1.01      3.4±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    430.0±0.51µs     6.9 MB/sec    1.00    430.5±3.23µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.01      5.9±0.03ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.1 MB/sec    1.01      6.7±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1481.8±2.80µs    11.2 MB/sec    1.00   1472.5±1.55µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.02    167.5±0.67µs    17.6 MB/sec    1.00    164.2±0.36µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.4 MB/sec    1.00      3.1±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.42ms     4.3 MB/sec    1.02      9.6±0.32ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.13ms     7.6 MB/sec    1.00      2.2±0.11ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00   262.1±16.29µs    11.3 MB/sec    1.02   267.5±18.36µs    11.0 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.20ms     5.3 MB/sec    1.04      5.0±0.24ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.00     18.5±0.70ms     2.2 MB/sec    1.00     18.4±0.55ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.9±0.17ms     3.4 MB/sec    1.00      4.9±0.22ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.01   602.8±35.00µs     4.9 MB/sec    1.00   596.8±27.93µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.3±0.34ms     3.1 MB/sec    1.00      8.1±0.31ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.7±0.37ms     4.2 MB/sec    1.00      9.7±0.38ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.07ms     8.0 MB/sec    1.01      2.1±0.17ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.01   241.6±17.77µs    12.2 MB/sec    1.00   238.1±10.32µs    12.4 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.4±0.30ms     5.8 MB/sec    1.00      4.3±0.19ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-25 22:16_

---

_Closed by @charliermarsh on 2023-06-25 22:16_

---

_Branch deleted on 2023-06-25 22:17_

---
