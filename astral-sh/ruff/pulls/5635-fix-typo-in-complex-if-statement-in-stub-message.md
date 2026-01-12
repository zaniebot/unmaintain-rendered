```yaml
number: 5635
title: Fix typo in complex-if-statement-in-stub message
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/typos
created_at: 2023-07-10T02:26:37Z
updated_at: 2023-07-10T02:57:29Z
url: https://github.com/astral-sh/ruff/pull/5635
synced_at: 2026-01-12T03:36:55Z
```

# Fix typo in complex-if-statement-in-stub message

---

_Pull request opened by @charliermarsh on 2023-07-10 02:26_

_No description provided._

---

_Merged by @charliermarsh on 2023-07-10 02:35_

---

_Closed by @charliermarsh on 2023-07-10 02:35_

---

_Branch deleted on 2023-07-10 02:35_

---

_Comment by @github-actions[bot] on 2023-07-10 02:57_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.2±0.02ms     5.0 MB/sec    1.00      8.2±0.03ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   1757.0±3.45µs     9.5 MB/sec    1.00  1757.5±13.19µs     9.5 MB/sec
formatter/numpy/globals.py                 1.00    195.5±1.53µs    15.1 MB/sec    1.00    196.3±0.22µs    15.0 MB/sec
formatter/pydantic/types.py                1.02      4.0±0.02ms     6.4 MB/sec    1.00      3.9±0.02ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.00     14.2±0.06ms     2.9 MB/sec    1.00     14.1±0.07ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.04ms     4.6 MB/sec    1.02      3.7±0.03ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    438.3±0.75µs     6.7 MB/sec    1.01    441.7±0.98µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.02ms     4.0 MB/sec    1.00      6.4±0.05ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.05ms     5.8 MB/sec    1.02      7.2±0.04ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1485.0±6.00µs    11.2 MB/sec    1.00   1479.2±8.30µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    168.6±0.29µs    17.5 MB/sec    1.01    170.3±0.85µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.1 MB/sec    1.00      3.1±0.02ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04      9.7±0.08ms     4.2 MB/sec    1.00      9.3±0.09ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.1±0.03ms     7.9 MB/sec    1.00      2.0±0.03ms     8.2 MB/sec
formatter/numpy/globals.py                 1.06   247.2±60.30µs    11.9 MB/sec    1.00    233.7±7.78µs    12.6 MB/sec
formatter/pydantic/types.py                1.03      4.7±0.07ms     5.5 MB/sec    1.00      4.5±0.06ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.04     16.2±0.15ms     2.5 MB/sec    1.00     15.5±0.14ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.05ms     4.0 MB/sec    1.00      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01   507.0±15.54µs     5.8 MB/sec    1.00    503.8±7.24µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.1±0.07ms     3.6 MB/sec    1.00      6.9±0.09ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.0±0.06ms     5.1 MB/sec    1.00      7.9±0.05ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1724.5±20.22µs     9.7 MB/sec    1.00  1709.9±18.85µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.01    205.9±3.29µs    14.3 MB/sec    1.00    203.5±2.73µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.04ms     7.0 MB/sec    1.00      3.6±0.06ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
