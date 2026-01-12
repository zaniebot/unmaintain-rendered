```yaml
number: 4815
title: Remove regex from partial-path rule
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/regex
created_at: 2023-06-02T18:17:02Z
updated_at: 2023-06-02T18:48:01Z
url: https://github.com/astral-sh/ruff/pull/4815
synced_at: 2026-01-12T15:55:16Z
```

# Remove regex from partial-path rule

---

_@charliermarsh_

_No description provided._

---

_Merged by @charliermarsh on 2023-06-02 18:28_

---

_Closed by @charliermarsh on 2023-06-02 18:28_

---

_Branch deleted on 2023-06-02 18:28_

---

_Comment by @github-actions[bot] on 2023-06-02 18:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.2±0.17ms     2.4 MB/sec    1.00     17.1±0.24ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.1 MB/sec    1.00      4.1±0.01ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.04   530.9±27.53µs     5.6 MB/sec    1.00    509.3±5.54µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.3±0.25ms     3.5 MB/sec    1.00      7.2±0.04ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.09ms     5.0 MB/sec    1.02      8.4±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1798.2±6.18µs     9.3 MB/sec    1.01   1815.4±3.99µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.04    217.0±9.94µs    13.6 MB/sec    1.00    208.3±2.95µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     6.8 MB/sec    1.00      3.7±0.03ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.2±0.06ms     6.6 MB/sec    1.00      6.2±0.01ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.01   1226.7±0.99µs    13.6 MB/sec    1.00   1216.9±3.09µs    13.7 MB/sec
parser/numpy/globals.py                    1.11   134.4±16.78µs    22.0 MB/sec    1.00    120.7±1.55µs    24.5 MB/sec
parser/pydantic/types.py                   1.03      2.7±0.01ms     9.5 MB/sec    1.00      2.6±0.05ms     9.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.1±0.04ms     2.7 MB/sec    1.00     15.2±0.04ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    392.0±1.87µs     7.5 MB/sec    1.01    394.0±4.57µs     7.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.06ms     4.1 MB/sec    1.00      6.2±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.02ms     5.4 MB/sec    1.00      7.6±0.02ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1541.7±6.48µs    10.8 MB/sec    1.01   1552.7±9.69µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    171.0±1.21µs    17.3 MB/sec    1.00    171.7±1.17µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.02ms     7.7 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
parser/large/dataset.py                    1.00      6.1±0.02ms     6.7 MB/sec    1.01      6.2±0.02ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.01   1134.7±3.16µs    14.7 MB/sec    1.00   1120.2±2.95µs    14.9 MB/sec
parser/numpy/globals.py                    1.05    117.4±0.42µs    25.1 MB/sec    1.00    112.1±0.61µs    26.3 MB/sec
parser/pydantic/types.py                   1.00      2.6±0.01ms    10.0 MB/sec    1.00      2.5±0.01ms    10.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
