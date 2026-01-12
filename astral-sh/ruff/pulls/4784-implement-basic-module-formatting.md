```yaml
number: 4784
title: Implement basic module formatting 
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: basic-module-formatter
created_at: 2023-06-01T12:39:38Z
updated_at: 2023-06-01T13:25:51Z
url: https://github.com/astral-sh/ruff/pull/4784
synced_at: 2026-01-12T03:50:03Z
```

# Implement basic module formatting 

---

_Pull request opened by @konstin on 2023-06-01 12:39_

This implements formatting each statement in a module with a hard line break in between, so that we can start formatting statements. It also adds Format for Stmt in the process.

Basic testing is done by the snapshots.

---

_Comment by @github-actions[bot] on 2023-06-01 12:51_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.5±0.09ms     2.3 MB/sec    1.00     17.3±0.14ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.2±0.05ms     3.9 MB/sec    1.00      4.1±0.03ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    509.2±2.01µs     5.8 MB/sec    1.00    507.4±0.70µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.04      7.4±0.03ms     3.5 MB/sec    1.00      7.1±0.05ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.4±0.04ms     4.9 MB/sec    1.00      8.3±0.04ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1806.0±19.97µs     9.2 MB/sec    1.00   1802.3±2.86µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.7±0.37µs    14.5 MB/sec    1.01    206.0±1.96µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.11ms     6.8 MB/sec    1.00      3.7±0.01ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.2±0.02ms     6.5 MB/sec    1.00      6.2±0.02ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.00   1214.4±5.66µs    13.7 MB/sec    1.00   1214.2±2.30µs    13.7 MB/sec
parser/numpy/globals.py                    1.00    125.4±1.21µs    23.5 MB/sec    1.00    125.1±0.33µs    23.6 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.00ms     9.6 MB/sec    1.00      2.7±0.00ms     9.6 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.00     20.9±0.56ms  1992.9 KB/sec     1.01     21.1±0.62ms  1976.3 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.3±0.19ms     3.1 MB/sec     1.00      5.3±0.19ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   617.7±22.18µs     4.8 MB/sec     1.02   630.0±33.39µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.03      9.3±0.69ms     2.7 MB/sec     1.00      9.1±0.40ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.02     10.3±0.26ms     3.9 MB/sec     1.00     10.2±0.31ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.2±0.08ms     7.5 MB/sec     1.00      2.2±0.07ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    254.5±8.63µs    11.6 MB/sec     1.01   256.6±14.74µs    11.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.7±0.17ms     5.4 MB/sec     1.00      4.7±0.18ms     5.5 MB/sec
parser/large/dataset.py                    1.00      8.1±0.28ms     5.0 MB/sec     1.10      8.9±0.19ms     4.6 MB/sec
parser/numpy/ctypeslib.py                  1.00  1564.5±106.45µs    10.6 MB/sec    1.06  1656.6±54.60µs    10.1 MB/sec
parser/numpy/globals.py                    1.00    158.7±8.91µs    18.6 MB/sec     1.04    165.2±8.03µs    17.9 MB/sec
parser/pydantic/types.py                   1.00      3.4±0.11ms     7.5 MB/sec     1.09      3.7±0.10ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-06-01 13:00_

---

_Merged by @konstin on 2023-06-01 13:25_

---

_Closed by @konstin on 2023-06-01 13:25_

---

_Branch deleted on 2023-06-01 13:25_

---
