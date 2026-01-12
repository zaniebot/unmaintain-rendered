```yaml
number: 5001
title: Parenthesize expressions prior to lexing in F632
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/paren
created_at: 2023-06-10T04:11:11Z
updated_at: 2023-06-10T04:48:00Z
url: https://github.com/astral-sh/ruff/pull/5001
synced_at: 2026-01-12T03:43:29Z
```

# Parenthesize expressions prior to lexing in F632

---

_Pull request opened by @charliermarsh on 2023-06-10 04:11_

Closes #5000.


---

_Label `bug` added by @charliermarsh on 2023-06-10 04:11_

---

_Merged by @charliermarsh on 2023-06-10 04:23_

---

_Closed by @charliermarsh on 2023-06-10 04:23_

---

_Branch deleted on 2023-06-10 04:23_

---

_Comment by @github-actions[bot] on 2023-06-10 04:29_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.01ms     6.0 MB/sec    1.05      7.1±0.01ms     5.7 MB/sec
formatter/numpy/ctypeslib.py               1.00   1394.3±7.23µs    11.9 MB/sec    1.03   1433.1±2.49µs    11.6 MB/sec
formatter/numpy/globals.py                 1.00    137.0±0.48µs    21.5 MB/sec    1.03    140.5±0.14µs    21.0 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.1 MB/sec    1.03      2.9±0.00ms     8.9 MB/sec
linter/all-rules/large/dataset.py          1.00     14.7±0.04ms     2.8 MB/sec    1.00     14.6±0.05ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.03ms     4.7 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    369.2±1.04µs     8.0 MB/sec    1.00    366.0±0.94µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.01ms     5.5 MB/sec    1.00      7.3±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1535.6±4.78µs    10.8 MB/sec    1.00   1530.4±4.86µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.5±0.29µs    17.9 MB/sec    1.00    164.7±0.26µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.00ms     7.7 MB/sec    1.00      3.3±0.00ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.07      8.2±0.10ms     5.0 MB/sec    1.00      7.6±0.14ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.07  1677.7±42.19µs     9.9 MB/sec    1.00  1561.6±16.00µs    10.7 MB/sec
formatter/numpy/globals.py                 1.03    158.4±4.99µs    18.6 MB/sec    1.00    153.2±7.86µs    19.3 MB/sec
formatter/pydantic/types.py                1.11      3.5±0.13ms     7.3 MB/sec    1.00      3.1±0.04ms     8.1 MB/sec
linter/all-rules/large/dataset.py          1.00     16.4±0.31ms     2.5 MB/sec    1.00     16.4±0.42ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.12ms     4.0 MB/sec    1.04      4.4±0.24ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   507.7±16.36µs     5.8 MB/sec    1.02   515.3±19.62µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.3±0.30ms     3.5 MB/sec    1.00      7.2±0.27ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.09ms     4.9 MB/sec    1.00      8.3±0.25ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1744.2±20.24µs     9.5 MB/sec    1.00  1736.1±14.46µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.02    201.1±3.99µs    14.7 MB/sec    1.00    197.6±2.84µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.05ms     6.8 MB/sec    1.00      3.7±0.04ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
