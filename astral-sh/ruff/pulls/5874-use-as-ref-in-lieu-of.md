```yaml
number: 5874
title: "Use `.as_ref()` in lieu of `&**`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/ref
created_at: 2023-07-19T00:42:40Z
updated_at: 2023-07-19T01:18:18Z
url: https://github.com/astral-sh/ruff/pull/5874
synced_at: 2026-01-12T03:30:22Z
```

# Use `.as_ref()` in lieu of `&**`

---

_Pull request opened by @charliermarsh on 2023-07-19 00:42_

I find this less opaque (and often more succinct).

---

_Merged by @charliermarsh on 2023-07-19 00:49_

---

_Closed by @charliermarsh on 2023-07-19 00:49_

---

_Branch deleted on 2023-07-19 00:49_

---

_Comment by @github-actions[bot] on 2023-07-19 00:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.0±0.08ms     3.7 MB/sec    1.00     11.0±0.03ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.02ms     7.6 MB/sec    1.00      2.2±0.00ms     7.5 MB/sec
formatter/numpy/globals.py                 1.00    247.2±1.78µs    11.9 MB/sec    1.00    248.3±0.69µs    11.9 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.03ms     5.4 MB/sec    1.01      4.8±0.01ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.05ms     2.6 MB/sec    1.00     15.3±0.09ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.01ms     4.2 MB/sec    1.00      3.9±0.01ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    517.8±2.08µs     5.7 MB/sec    1.00    513.9±5.39µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.02ms     3.6 MB/sec    1.00      6.9±0.05ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.02ms     5.1 MB/sec    1.00      7.9±0.05ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1685.7±3.04µs     9.9 MB/sec    1.00   1669.3±9.60µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    184.5±0.96µs    16.0 MB/sec    1.00    184.5±3.45µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.02ms     7.2 MB/sec    1.00      3.5±0.06ms     7.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     14.0±0.54ms     2.9 MB/sec    1.12     15.7±0.69ms     2.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.7±0.12ms     6.2 MB/sec    1.08      2.9±0.15ms     5.7 MB/sec
formatter/numpy/globals.py                 1.00   313.5±15.43µs     9.4 MB/sec    1.07   334.0±16.82µs     8.8 MB/sec
formatter/pydantic/types.py                1.00      5.9±0.30ms     4.3 MB/sec    1.06      6.2±0.25ms     4.1 MB/sec
linter/all-rules/large/dataset.py          1.00     19.9±0.60ms     2.0 MB/sec    1.04     20.7±0.73ms  2009.8 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.3±0.26ms     3.2 MB/sec    1.05      5.5±0.27ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   631.5±31.58µs     4.7 MB/sec    1.04   656.1±28.39µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.2±0.38ms     2.8 MB/sec    1.04      9.5±0.43ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.00     10.9±0.43ms     3.7 MB/sec    1.03     11.2±0.50ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.09ms     7.6 MB/sec    1.03      2.3±0.10ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   262.5±13.67µs    11.2 MB/sec    1.05   276.0±11.78µs    10.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.20ms     5.3 MB/sec    1.01      4.9±0.27ms     5.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
