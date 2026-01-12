```yaml
number: 4402
title: "Replace `macro_rules!` visitors with dedicated methods"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/macro
created_at: 2023-05-12T20:50:01Z
updated_at: 2023-05-12T21:20:31Z
url: https://github.com/astral-sh/ruff/pull/4402
synced_at: 2026-01-12T15:55:15Z
```

# Replace `macro_rules!` visitors with dedicated methods

---

_@charliermarsh_

These aren't necessary. It's simpler to use methods.

---

_@MichaReiser approved on 2023-05-12 20:59_

Thx

---

_Merged by @charliermarsh on 2023-05-12 21:05_

---

_Closed by @charliermarsh on 2023-05-12 21:06_

---

_Branch deleted on 2023-05-12 21:06_

---

_Comment by @github-actions[bot] on 2023-05-12 21:14_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.07ms     2.4 MB/sec    1.00     16.7±0.08ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.01ms     4.1 MB/sec    1.00      4.0±0.02ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    494.1±1.07µs     6.0 MB/sec    1.00    492.0±2.08µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.04ms     3.7 MB/sec    1.00      6.9±0.03ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.03ms     5.0 MB/sec    1.00      8.1±0.02ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1754.3±41.15µs     9.5 MB/sec    1.00   1738.7±3.04µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.5±0.46µs    15.4 MB/sec    1.00    192.3±1.03µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.01ms     7.0 MB/sec    1.00      3.6±0.02ms     7.0 MB/sec
parser/large/dataset.py                    1.09      7.0±0.01ms     5.8 MB/sec    1.00      6.5±0.01ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.07   1340.9±0.91µs    12.4 MB/sec    1.00   1257.3±1.03µs    13.2 MB/sec
parser/numpy/globals.py                    1.06    134.5±0.22µs    21.9 MB/sec    1.00    127.3±0.22µs    23.2 MB/sec
parser/pydantic/types.py                   1.07      3.0±0.00ms     8.6 MB/sec    1.00      2.8±0.00ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     19.9±0.61ms     2.0 MB/sec    1.03     20.6±0.66ms  2025.8 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.21ms     3.3 MB/sec    1.02      5.2±0.16ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   582.0±23.27µs     5.1 MB/sec    1.05   611.7±28.90µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.2±0.27ms     3.1 MB/sec    1.04      8.5±0.27ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00     10.0±0.26ms     4.1 MB/sec    1.01     10.0±0.21ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.07ms     8.1 MB/sec    1.04      2.1±0.09ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   238.0±10.26µs    12.4 MB/sec    1.01   241.1±19.54µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.03      4.6±0.19ms     5.6 MB/sec    1.00      4.4±0.12ms     5.8 MB/sec
parser/large/dataset.py                    1.00      8.1±0.22ms     5.0 MB/sec    1.02      8.2±0.18ms     5.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1508.0±45.80µs    11.0 MB/sec    1.05  1588.6±60.95µs    10.5 MB/sec
parser/numpy/globals.py                    1.00    156.5±6.61µs    18.9 MB/sec    1.02   159.8±12.25µs    18.5 MB/sec
parser/pydantic/types.py                   1.00      3.4±0.09ms     7.5 MB/sec    1.03      3.5±0.08ms     7.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
