```yaml
number: 4393
title: Add PyTorch to user list
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/torch
created_at: 2023-05-12T17:55:52Z
updated_at: 2023-05-12T18:29:34Z
url: https://github.com/astral-sh/ruff/pull/4393
synced_at: 2026-01-12T03:50:02Z
```

# Add PyTorch to user list

---

_Pull request opened by @charliermarsh on 2023-05-12 17:55_

_No description provided._

---

_Merged by @charliermarsh on 2023-05-12 18:02_

---

_Closed by @charliermarsh on 2023-05-12 18:02_

---

_Branch deleted on 2023-05-12 18:02_

---

_Comment by @github-actions[bot] on 2023-05-12 18:06_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.1±0.20ms     2.9 MB/sec    1.00     13.9±0.13ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.02ms     4.9 MB/sec    1.00      3.3±0.02ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    414.5±1.82µs     7.1 MB/sec    1.00    410.4±0.59µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.06ms     4.4 MB/sec    1.00      5.8±0.07ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.03ms     6.0 MB/sec    1.00      6.7±0.03ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1439.7±3.93µs    11.6 MB/sec    1.00   1438.9±3.66µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    160.7±0.76µs    18.4 MB/sec    1.01    161.6±2.78µs    18.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
parser/large/dataset.py                    1.02      5.5±0.01ms     7.3 MB/sec    1.00      5.4±0.01ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1051.9±0.89µs    15.8 MB/sec    1.00   1056.6±2.09µs    15.8 MB/sec
parser/numpy/globals.py                    1.00    107.9±0.31µs    27.3 MB/sec    1.00    107.6±0.17µs    27.4 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.1 MB/sec    1.00      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.2±0.31ms     2.5 MB/sec    1.01     16.3±0.37ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.08ms     4.0 MB/sec    1.01      4.2±0.11ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   492.4±14.15µs     6.0 MB/sec    1.01   495.2±12.96µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.15ms     3.7 MB/sec    1.00      6.8±0.13ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.11ms     5.0 MB/sec    1.01      8.2±0.10ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1709.8±24.98µs     9.7 MB/sec    1.03  1757.7±49.37µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    193.2±4.81µs    15.3 MB/sec    1.03    199.1±6.61µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.05ms     7.1 MB/sec    1.03      3.7±0.10ms     6.9 MB/sec
parser/large/dataset.py                    1.01      6.7±0.13ms     6.1 MB/sec    1.00      6.7±0.11ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.01  1260.1±19.53µs    13.2 MB/sec    1.00  1245.7±15.13µs    13.4 MB/sec
parser/numpy/globals.py                    1.01    129.6±4.87µs    22.8 MB/sec    1.00    128.4±3.73µs    23.0 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.1 MB/sec    1.01      2.8±0.08ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
