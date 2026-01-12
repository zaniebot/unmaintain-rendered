```yaml
number: 4003
title: Fix defaults for section-order
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/default
created_at: 2023-04-18T02:54:59Z
updated_at: 2023-04-18T03:24:45Z
url: https://github.com/astral-sh/ruff/pull/4003
synced_at: 2026-01-12T15:55:14Z
```

# Fix defaults for section-order

---

_@charliermarsh_

_No description provided._

---

_Merged by @charliermarsh on 2023-04-18 03:00_

---

_Closed by @charliermarsh on 2023-04-18 03:00_

---

_Branch deleted on 2023-04-18 03:00_

---

_Comment by @github-actions[bot] on 2023-04-18 03:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.3±0.05ms     2.7 MB/sec    1.00     15.3±0.05ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.01ms     4.3 MB/sec    1.00      3.9±0.01ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    420.2±1.99µs     7.0 MB/sec    1.00    420.0±3.80µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.03ms     3.9 MB/sec    1.00      6.5±0.02ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.01      7.9±0.02ms     5.1 MB/sec    1.00      7.9±0.03ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1752.6±3.76µs     9.5 MB/sec    1.01   1765.0±6.49µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    182.7±2.62µs    16.2 MB/sec    1.01    183.8±0.87µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.0 MB/sec    1.01      3.7±0.01ms     7.0 MB/sec
parser/large/dataset.py                    1.01      6.2±0.01ms     6.6 MB/sec    1.00      6.2±0.02ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.00  1236.5±13.58µs    13.5 MB/sec    1.00   1236.2±5.45µs    13.5 MB/sec
parser/numpy/globals.py                    1.00    124.2±0.25µs    23.8 MB/sec    1.01    125.0±0.58µs    23.6 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.00ms     9.5 MB/sec    1.00      2.7±0.00ms     9.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.5±0.34ms     2.5 MB/sec    1.01     16.6±0.39ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.09ms     3.9 MB/sec    1.03      4.4±0.20ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   449.5±14.76µs     6.6 MB/sec    1.02   458.8±17.13µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.1±0.18ms     3.6 MB/sec    1.00      7.1±0.14ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.11ms     4.9 MB/sec    1.01      8.4±0.10ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1835.4±57.30µs     9.1 MB/sec    1.00  1832.0±31.89µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.3±9.52µs    15.4 MB/sec    1.02    195.1±9.39µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.10ms     6.6 MB/sec    1.00      3.9±0.12ms     6.6 MB/sec
parser/large/dataset.py                    1.00      6.7±0.12ms     6.0 MB/sec    1.00      6.7±0.09ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1286.8±31.26µs    12.9 MB/sec    1.00  1291.0±34.66µs    12.9 MB/sec
parser/numpy/globals.py                    1.01    129.5±2.62µs    22.8 MB/sec    1.00    128.8±2.71µs    22.9 MB/sec
parser/pydantic/types.py                   1.01      2.9±0.06ms     8.9 MB/sec    1.00      2.9±0.05ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
