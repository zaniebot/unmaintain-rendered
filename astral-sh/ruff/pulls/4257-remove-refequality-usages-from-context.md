```yaml
number: 4257
title: "Remove `RefEquality` usages from `Context`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/ref
created_at: 2023-05-06T19:30:57Z
updated_at: 2023-05-06T20:03:53Z
url: https://github.com/astral-sh/ruff/pull/4257
synced_at: 2026-01-12T15:55:15Z
```

# Remove `RefEquality` usages from `Context`

---

_@charliermarsh_

## Summary

No longer any use for these.

---

_Comment by @github-actions[bot] on 2023-05-06 19:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.3±0.28ms     2.5 MB/sec    1.01     16.5±0.26ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.08ms     4.2 MB/sec    1.00      3.9±0.10ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   486.8±13.33µs     6.1 MB/sec    1.01    493.2±7.94µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.8±0.15ms     3.8 MB/sec    1.00      6.8±0.17ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.15ms     5.1 MB/sec    1.01      8.0±0.15ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1734.0±36.44µs     9.6 MB/sec    1.00  1730.0±35.61µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.2±5.48µs    15.6 MB/sec    1.01    190.6±6.67µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.09ms     7.2 MB/sec    1.02      3.6±0.08ms     7.1 MB/sec
parser/large/dataset.py                    1.00      6.3±0.17ms     6.5 MB/sec    1.00      6.3±0.17ms     6.4 MB/sec
parser/numpy/ctypeslib.py                  1.00  1228.8±36.57µs    13.6 MB/sec    1.01  1246.7±24.42µs    13.4 MB/sec
parser/numpy/globals.py                    1.01    125.2±3.30µs    23.6 MB/sec    1.00    124.1±6.11µs    23.8 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.07ms     9.5 MB/sec    1.01      2.7±0.05ms     9.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     17.0±0.22ms     2.4 MB/sec    1.00     16.3±0.21ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.2±0.07ms     3.9 MB/sec    1.00      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.04    497.0±6.93µs     5.9 MB/sec    1.00    479.8±5.56µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.04      7.1±0.07ms     3.6 MB/sec    1.00      6.8±0.08ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.08      8.9±0.09ms     4.6 MB/sec    1.00      8.2±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.08  1854.9±20.64µs     9.0 MB/sec    1.00  1724.8±21.97µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.06    206.0±3.93µs    14.3 MB/sec    1.00    194.1±6.63µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.07      3.9±0.05ms     6.5 MB/sec    1.00      3.7±0.08ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.6±0.05ms     6.2 MB/sec    1.01      6.6±0.05ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1239.3±12.89µs    13.4 MB/sec    1.02  1261.5±17.42µs    13.2 MB/sec
parser/numpy/globals.py                    1.00    127.4±3.04µs    23.2 MB/sec    1.00    127.8±1.70µs    23.1 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.3 MB/sec    1.01      2.8±0.02ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-06 19:55_

---

_Closed by @charliermarsh on 2023-05-06 19:55_

---

_Branch deleted on 2023-05-06 19:55_

---
