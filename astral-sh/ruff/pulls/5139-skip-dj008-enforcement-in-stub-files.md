```yaml
number: 5139
title: "Skip `DJ008` enforcement in stub files"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/stub
created_at: 2023-06-16T03:43:03Z
updated_at: 2023-06-16T04:09:18Z
url: https://github.com/astral-sh/ruff/pull/5139
synced_at: 2026-01-12T03:43:30Z
```

# Skip `DJ008` enforcement in stub files

---

_Pull request opened by @charliermarsh on 2023-06-16 03:43_

Closes #5138.

---

_Label `bug` added by @charliermarsh on 2023-06-16 03:43_

---

_Merged by @charliermarsh on 2023-06-16 03:49_

---

_Closed by @charliermarsh on 2023-06-16 03:49_

---

_Branch deleted on 2023-06-16 03:49_

---

_Comment by @github-actions[bot] on 2023-06-16 03:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.9±0.01ms     5.9 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.01   1406.5±1.94µs    11.8 MB/sec    1.00   1395.1±2.75µs    11.9 MB/sec
formatter/numpy/globals.py                 1.00    138.7±0.37µs    21.3 MB/sec    1.00    138.5±0.42µs    21.3 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.1 MB/sec    1.00      2.8±0.01ms     9.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.2±0.03ms     2.9 MB/sec    1.00     14.2±0.03ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.8 MB/sec    1.00      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    365.0±0.94µs     8.1 MB/sec    1.00    363.7±1.39µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.1±0.04ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.02ms     5.7 MB/sec    1.00      7.2±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1513.2±5.97µs    11.0 MB/sec    1.00   1504.4±3.09µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    164.5±0.78µs    17.9 MB/sec    1.00    163.4±0.33µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.8 MB/sec    1.00      3.3±0.02ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.19      9.0±0.06ms     4.5 MB/sec    1.00      7.6±0.07ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.13  1766.3±29.35µs     9.4 MB/sec    1.00  1562.3±22.35µs    10.7 MB/sec
formatter/numpy/globals.py                 1.09    164.5±3.59µs    17.9 MB/sec    1.00    150.5±8.13µs    19.6 MB/sec
formatter/pydantic/types.py                1.16      3.6±0.04ms     7.1 MB/sec    1.00      3.1±0.05ms     8.2 MB/sec
linter/all-rules/large/dataset.py          1.00     16.0±0.08ms     2.5 MB/sec    1.00     16.0±0.16ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.0 MB/sec    1.00      4.1±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    494.4±6.21µs     6.0 MB/sec    1.00    491.0±6.08µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.08ms     3.7 MB/sec    1.00      7.0±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.08ms     5.0 MB/sec    1.00      8.2±0.04ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1735.4±17.37µs     9.6 MB/sec    1.00  1726.7±17.86µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    200.4±4.29µs    14.7 MB/sec    1.00    199.4±6.02µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.9 MB/sec    1.01      3.7±0.03ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
