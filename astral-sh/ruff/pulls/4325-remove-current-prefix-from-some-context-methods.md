```yaml
number: 4325
title: "Remove `current_` prefix from some Context methods"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/current
created_at: 2023-05-09T19:35:10Z
updated_at: 2023-05-09T20:05:43Z
url: https://github.com/astral-sh/ruff/pull/4325
synced_at: 2026-01-12T03:56:39Z
```

# Remove `current_` prefix from some Context methods

---

_Pull request opened by @charliermarsh on 2023-05-09 19:35_

## Summary

I don't think `current_` adds much here. They're already clearly getters (they don't take any arguments).

---

_Merged by @charliermarsh on 2023-05-09 19:40_

---

_Closed by @charliermarsh on 2023-05-09 19:40_

---

_Branch deleted on 2023-05-09 19:40_

---

_Comment by @github-actions[bot] on 2023-05-09 19:45_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.7±0.34ms     2.3 MB/sec    1.01     17.9±0.38ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.12ms     4.0 MB/sec    1.03      4.3±0.21ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   530.0±22.30µs     5.6 MB/sec    1.01   535.1±17.16µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.5±0.37ms     3.4 MB/sec    1.00      7.4±0.18ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.13ms     4.9 MB/sec    1.02      8.5±0.13ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1796.4±68.96µs     9.3 MB/sec    1.01  1815.9±28.77µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    213.8±7.62µs    13.8 MB/sec    1.05   224.2±18.67µs    13.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.12ms     6.8 MB/sec    1.03      3.9±0.12ms     6.6 MB/sec
parser/large/dataset.py                    1.02      6.9±0.46ms     5.9 MB/sec    1.00      6.7±0.10ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1338.7±43.83µs    12.4 MB/sec    1.00  1332.6±30.63µs    12.5 MB/sec
parser/numpy/globals.py                    1.01    132.5±7.44µs    22.3 MB/sec    1.00    131.3±4.52µs    22.5 MB/sec
parser/pydantic/types.py                   1.02      3.0±0.13ms     8.6 MB/sec    1.00      2.9±0.07ms     8.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     24.6±0.99ms  1694.9 KB/sec    1.00     24.1±0.78ms  1729.9 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      6.1±0.25ms     2.7 MB/sec    1.00      5.9±0.24ms     2.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   690.8±39.91µs     4.3 MB/sec    1.00   691.1±35.57µs     4.3 MB/sec
linter/all-rules/pydantic/types.py         1.03     10.4±0.58ms     2.4 MB/sec    1.00     10.1±0.46ms     2.5 MB/sec
linter/default-rules/large/dataset.py      1.00     12.1±0.44ms     3.4 MB/sec    1.00     12.0±0.43ms     3.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.5±0.09ms     6.6 MB/sec    1.00      2.5±0.13ms     6.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   293.5±16.96µs    10.1 MB/sec    1.02   300.6±15.15µs     9.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.3±0.37ms     4.8 MB/sec    1.01      5.4±0.27ms     4.7 MB/sec
parser/large/dataset.py                    1.04     10.3±0.36ms     3.9 MB/sec    1.00      9.9±0.50ms     4.1 MB/sec
parser/numpy/ctypeslib.py                  1.04  1945.3±72.96µs     8.6 MB/sec    1.00  1863.3±63.79µs     8.9 MB/sec
parser/numpy/globals.py                    1.06   198.4±14.47µs    14.9 MB/sec    1.00    186.8±8.67µs    15.8 MB/sec
parser/pydantic/types.py                   1.04      4.3±0.17ms     5.9 MB/sec    1.00      4.2±0.27ms     6.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
