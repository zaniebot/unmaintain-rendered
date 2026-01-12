```yaml
number: 6666
title: Remove some trailing commas in write calls
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/trailing-comma
created_at: 2023-08-18T04:03:02Z
updated_at: 2023-08-18T04:32:39Z
url: https://github.com/astral-sh/ruff/pull/6666
synced_at: 2026-01-12T02:52:04Z
```

# Remove some trailing commas in write calls

---

_Pull request opened by @charliermarsh on 2023-08-18 04:03_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-08-18 04:03_

---

_Merged by @charliermarsh on 2023-08-18 04:14_

---

_Closed by @charliermarsh on 2023-08-18 04:14_

---

_Branch deleted on 2023-08-18 04:14_

---

_Comment by @github-actions[bot] on 2023-08-18 04:17_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      4.0±0.02ms    10.1 MB/sec    1.00      4.0±0.03ms    10.2 MB/sec
formatter/numpy/ctypeslib.py               1.01   850.8±10.46µs    19.6 MB/sec    1.00    845.9±4.08µs    19.7 MB/sec
formatter/numpy/globals.py                 1.01     91.4±0.52µs    32.3 MB/sec    1.00     90.2±0.50µs    32.7 MB/sec
formatter/pydantic/types.py                1.00   1651.1±8.36µs    15.4 MB/sec    1.00  1651.7±23.72µs    15.4 MB/sec
linter/all-rules/large/dataset.py          1.00     12.2±0.09ms     3.3 MB/sec    1.00     12.2±0.05ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.1 MB/sec    1.01      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    462.2±2.18µs     6.4 MB/sec    1.01    465.0±1.38µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.06ms     4.0 MB/sec    1.01      6.4±0.03ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.03ms     6.3 MB/sec    1.00      6.5±0.02ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1427.5±6.41µs    11.7 MB/sec    1.00   1432.0±3.86µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    167.8±5.05µs    17.6 MB/sec    1.00    167.8±1.31µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.02ms     8.8 MB/sec    1.01      2.9±0.03ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.7±0.05ms    11.0 MB/sec    1.02      3.8±0.06ms    10.8 MB/sec
formatter/numpy/ctypeslib.py               1.00    761.5±8.44µs    21.9 MB/sec    1.01   770.0±11.56µs    21.6 MB/sec
formatter/numpy/globals.py                 1.00     80.4±1.52µs    36.7 MB/sec    1.00     80.3±3.66µs    36.7 MB/sec
formatter/pydantic/types.py                1.00  1525.6±32.87µs    16.7 MB/sec    1.02  1555.4±25.30µs    16.4 MB/sec
linter/all-rules/large/dataset.py          1.00     12.5±0.09ms     3.3 MB/sec    1.01     12.6±0.09ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.03ms     4.8 MB/sec    1.01      3.5±0.04ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    435.2±4.55µs     6.8 MB/sec    1.01    439.0±4.71µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.08ms     3.9 MB/sec    1.00      6.5±0.06ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.05ms     5.9 MB/sec    1.00      6.9±0.04ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1471.6±16.42µs    11.3 MB/sec    1.01  1484.6±21.12µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.6±2.68µs    17.0 MB/sec    1.01    175.9±2.44µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.3 MB/sec    1.01      3.1±0.03ms     8.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
