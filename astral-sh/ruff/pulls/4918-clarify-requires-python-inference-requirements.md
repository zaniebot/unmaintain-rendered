```yaml
number: 4918
title: Clarify requires-python inference requirements
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/target-version
created_at: 2023-06-07T04:12:01Z
updated_at: 2023-06-07T04:42:14Z
url: https://github.com/astral-sh/ruff/pull/4918
synced_at: 2026-01-12T03:43:29Z
```

# Clarify requires-python inference requirements

---

_Pull request opened by @charliermarsh on 2023-06-07 04:12_

Closes #4902.

---

_Label `documentation` added by @charliermarsh on 2023-06-07 04:12_

---

_Merged by @charliermarsh on 2023-06-07 04:18_

---

_Closed by @charliermarsh on 2023-06-07 04:18_

---

_Branch deleted on 2023-06-07 04:18_

---

_Comment by @github-actions[bot] on 2023-06-07 04:22_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.2±0.01ms     6.6 MB/sec    1.01      6.2±0.01ms     6.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   1262.5±2.06µs    13.2 MB/sec    1.01   1272.4±2.83µs    13.1 MB/sec
formatter/numpy/globals.py                 1.00    146.0±0.31µs    20.2 MB/sec    1.01    146.8±0.48µs    20.1 MB/sec
formatter/pydantic/types.py                1.00      2.7±0.00ms     9.4 MB/sec    1.01      2.7±0.01ms     9.4 MB/sec
linter/all-rules/large/dataset.py          1.00     14.9±0.06ms     2.7 MB/sec    1.01     15.1±0.04ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    365.2±1.24µs     8.1 MB/sec    1.00    364.1±0.96µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.02ms     4.1 MB/sec    1.01      6.3±0.03ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.02ms     5.5 MB/sec    1.01      7.5±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1531.4±4.23µs    10.9 MB/sec    1.01   1553.4±1.88µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.6±0.89µs    17.9 MB/sec    1.01    165.6±0.25µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.00ms     7.8 MB/sec    1.01      3.3±0.01ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.0±0.06ms     5.8 MB/sec    1.00      7.0±0.07ms     5.8 MB/sec
formatter/numpy/ctypeslib.py               1.01  1425.8±25.22µs    11.7 MB/sec    1.00  1414.7±19.41µs    11.8 MB/sec
formatter/numpy/globals.py                 1.00    161.1±4.31µs    18.3 MB/sec    1.01    162.4±5.02µs    18.2 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.05ms     8.2 MB/sec    1.01      3.1±0.08ms     8.1 MB/sec
linter/all-rules/large/dataset.py          1.00     17.3±0.22ms     2.3 MB/sec    1.00     17.4±0.16ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.06ms     3.9 MB/sec    1.00      4.3±0.08ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    498.6±9.95µs     5.9 MB/sec    1.00    494.9±6.60µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.11ms     3.5 MB/sec    1.00      7.3±0.08ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.08ms     4.8 MB/sec    1.04      8.8±0.15ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1750.8±14.55µs     9.5 MB/sec    1.04  1817.0±21.17µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.4±6.28µs    14.8 MB/sec    1.02   204.2±22.34µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.8 MB/sec    1.03      3.9±0.03ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
