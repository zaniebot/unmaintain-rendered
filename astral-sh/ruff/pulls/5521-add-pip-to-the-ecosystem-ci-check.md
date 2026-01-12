```yaml
number: 5521
title: Add pip to the ecosystem-ci check
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/ecosystem
created_at: 2023-07-05T02:00:49Z
updated_at: 2023-07-05T02:33:34Z
url: https://github.com/astral-sh/ruff/pull/5521
synced_at: 2026-01-12T03:36:55Z
```

# Add pip to the ecosystem-ci check

---

_Pull request opened by @charliermarsh on 2023-07-05 02:00_

_No description provided._

---

_Merged by @charliermarsh on 2023-07-05 02:06_

---

_Closed by @charliermarsh on 2023-07-05 02:06_

---

_Branch deleted on 2023-07-05 02:06_

---

_Comment by @github-actions[bot] on 2023-07-05 02:09_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.8±0.27ms     4.6 MB/sec    1.03      9.1±0.12ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1905.4±65.96µs     8.7 MB/sec    1.06      2.0±0.04ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00   209.6±11.92µs    14.1 MB/sec    1.08    226.1±4.21µs    13.1 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.16ms     6.1 MB/sec    1.04      4.4±0.06ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.00     14.7±0.43ms     2.8 MB/sec    1.00     14.8±0.41ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.12ms     4.5 MB/sec    1.00      3.7±0.12ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   475.3±18.94µs     6.2 MB/sec    1.02   485.4±20.15µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.20ms     3.9 MB/sec    1.00      6.6±0.19ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.21ms     5.4 MB/sec    1.00      7.4±0.19ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1651.7±67.86µs    10.1 MB/sec    1.05  1729.9±29.38µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    185.7±8.02µs    15.9 MB/sec    1.08    200.5±3.00µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.11ms     7.5 MB/sec    1.05      3.6±0.04ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04      9.9±0.46ms     4.1 MB/sec    1.00      9.5±0.27ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.1±0.13ms     7.9 MB/sec    1.00      2.0±0.04ms     8.1 MB/sec
formatter/numpy/globals.py                 1.00    226.5±4.57µs    13.0 MB/sec    1.02   231.9±12.51µs    12.7 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.10ms     5.6 MB/sec    1.00      4.5±0.09ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.03     16.2±0.48ms     2.5 MB/sec    1.00     15.8±0.23ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.2±0.09ms     4.0 MB/sec    1.00      4.1±0.07ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   505.5±11.79µs     5.8 MB/sec    1.00   503.1±10.93µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.07      7.4±0.31ms     3.5 MB/sec    1.00      6.9±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.14ms     5.0 MB/sec    1.00      8.1±0.12ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05  1808.4±57.50µs     9.2 MB/sec    1.00  1719.0±37.00µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.2±7.39µs    14.8 MB/sec    1.00    199.5±6.84µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.7±0.13ms     6.8 MB/sec    1.00      3.6±0.08ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
