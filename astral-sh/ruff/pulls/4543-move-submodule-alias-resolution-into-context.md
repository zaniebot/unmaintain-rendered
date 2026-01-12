```yaml
number: 4543
title: "Move submodule alias resolution into `Context`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/alias
created_at: 2023-05-20T16:26:12Z
updated_at: 2023-05-20T16:57:25Z
url: https://github.com/astral-sh/ruff/pull/4543
synced_at: 2026-01-12T03:50:03Z
```

# Move submodule alias resolution into `Context`

---

_Pull request opened by @charliermarsh on 2023-05-20 16:26_

_No description provided._

---

_Merged by @charliermarsh on 2023-05-20 16:34_

---

_Closed by @charliermarsh on 2023-05-20 16:34_

---

_Branch deleted on 2023-05-20 16:34_

---

_Comment by @github-actions[bot] on 2023-05-20 16:39_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     14.4±0.11ms     2.8 MB/sec    1.00     14.2±0.05ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.8 MB/sec    1.00      3.5±0.02ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    429.4±0.68µs     6.9 MB/sec    1.00    427.1±1.60µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.9±0.02ms     4.3 MB/sec    1.00      5.9±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.01ms     6.0 MB/sec    1.00      6.8±0.03ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1477.6±4.22µs    11.3 MB/sec    1.00   1471.5±1.36µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.5±0.21µs    17.9 MB/sec    1.00    164.8±1.42µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.00      5.4±0.00ms     7.6 MB/sec    1.00      5.4±0.02ms     7.6 MB/sec
parser/numpy/ctypeslib.py                  1.00   1056.2±1.12µs    15.8 MB/sec    1.00   1057.3±1.73µs    15.7 MB/sec
parser/numpy/globals.py                    1.00    109.2±2.32µs    27.0 MB/sec    1.00    109.1±0.18µs    27.1 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.1 MB/sec    1.00      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.6±0.10ms     2.5 MB/sec    1.00     16.6±0.10ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.04ms     3.9 MB/sec    1.00      4.3±0.04ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    434.8±4.15µs     6.8 MB/sec    1.00    435.0±3.93µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.03ms     3.6 MB/sec    1.00      7.1±0.03ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.07ms     4.8 MB/sec    1.00      8.5±0.02ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1727.1±10.55µs     9.6 MB/sec    1.02   1759.7±9.08µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    190.2±9.23µs    15.5 MB/sec    1.00    189.5±4.38µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.01ms     6.8 MB/sec    1.01      3.8±0.02ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.9±0.06ms     5.9 MB/sec    1.00      6.9±0.02ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1311.8±9.09µs    12.7 MB/sec    1.00  1310.3±12.51µs    12.7 MB/sec
parser/numpy/globals.py                    1.00    137.1±0.60µs    21.5 MB/sec    1.00    136.8±1.81µs    21.6 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.03ms     8.8 MB/sec    1.00      2.9±0.01ms     8.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
