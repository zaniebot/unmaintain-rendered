```yaml
number: 4570
title: Reduce visibility of more functions, structs, and fields
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/pub
created_at: 2023-05-22T03:30:05Z
updated_at: 2023-05-22T04:03:02Z
url: https://github.com/astral-sh/ruff/pull/4570
synced_at: 2026-01-12T15:55:15Z
```

# Reduce visibility of more functions, structs, and fields

---

_@charliermarsh_

_No description provided._

---

_Merged by @charliermarsh on 2023-05-22 03:36_

---

_Closed by @charliermarsh on 2023-05-22 03:36_

---

_Branch deleted on 2023-05-22 03:36_

---

_Comment by @github-actions[bot] on 2023-05-22 03:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.3±0.08ms     2.3 MB/sec    1.01     17.5±0.05ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.0 MB/sec    1.01      4.2±0.01ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    508.2±1.54µs     5.8 MB/sec    1.01    511.0±1.08µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.04ms     3.6 MB/sec    1.01      7.2±0.03ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.03ms     5.0 MB/sec    1.03      8.4±0.03ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1755.6±6.54µs     9.5 MB/sec    1.02  1798.0±65.90µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.3±0.69µs    15.1 MB/sec    1.54  300.3±324.19µs     9.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     7.0 MB/sec    1.42      5.2±2.27ms     4.9 MB/sec
parser/large/dataset.py                    1.00      6.5±0.01ms     6.2 MB/sec    1.00      6.5±0.01ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.02   1304.2±1.19µs    12.8 MB/sec    1.00   1279.8±0.85µs    13.0 MB/sec
parser/numpy/globals.py                    1.00    132.0±0.24µs    22.3 MB/sec    1.00    132.1±1.27µs    22.3 MB/sec
parser/pydantic/types.py                   1.02      2.8±0.00ms     9.0 MB/sec    1.00      2.8±0.01ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.6±0.11ms     2.4 MB/sec    1.01     16.8±0.08ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.05ms     3.9 MB/sec    1.00      4.3±0.02ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   428.1±12.83µs     6.9 MB/sec    1.00    427.8±4.61µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.05ms     3.6 MB/sec    1.00      7.1±0.03ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.02ms     4.9 MB/sec    1.01      8.5±0.02ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1722.7±13.58µs     9.7 MB/sec    1.02  1754.8±10.25µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.3±1.42µs    16.3 MB/sec    1.02    185.5±6.26µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.02ms     6.8 MB/sec    1.01      3.8±0.05ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.9±0.03ms     5.9 MB/sec    1.00      6.9±0.02ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1289.8±8.00µs    12.9 MB/sec    1.02  1314.8±18.80µs    12.7 MB/sec
parser/numpy/globals.py                    1.00    134.3±0.99µs    22.0 MB/sec    1.01    136.1±0.97µs    21.7 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.01ms     8.9 MB/sec    1.01      2.9±0.01ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
