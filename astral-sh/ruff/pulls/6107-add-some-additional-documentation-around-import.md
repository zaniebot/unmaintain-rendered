```yaml
number: 6107
title: Add some additional documentation around import categorization
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/package-docs
created_at: 2023-07-26T22:01:22Z
updated_at: 2023-07-26T23:09:09Z
url: https://github.com/astral-sh/ruff/pull/6107
synced_at: 2026-01-12T03:30:22Z
```

# Add some additional documentation around import categorization

---

_Pull request opened by @charliermarsh on 2023-07-26 22:01_

Closes https://github.com/astral-sh/ruff/issues/5529.

---

_Label `documentation` added by @charliermarsh on 2023-07-26 22:01_

---

_Merged by @charliermarsh on 2023-07-26 22:39_

---

_Closed by @charliermarsh on 2023-07-26 22:39_

---

_Branch deleted on 2023-07-26 22:39_

---

_Comment by @github-actions[bot] on 2023-07-26 22:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.1±0.09ms     4.0 MB/sec    1.00     10.1±0.05ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.04ms     8.3 MB/sec    1.00      2.0±0.04ms     8.3 MB/sec
formatter/numpy/globals.py                 1.00    225.7±5.64µs    13.1 MB/sec    1.03    233.4±6.40µs    12.6 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.05ms     5.9 MB/sec    1.00      4.3±0.09ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     14.2±0.06ms     2.9 MB/sec    1.09     15.4±0.12ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.6 MB/sec    1.06      3.8±0.07ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    470.6±1.01µs     6.3 MB/sec    1.03    484.9±0.76µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.07ms     4.0 MB/sec    1.07      6.9±0.10ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.03ms     5.8 MB/sec    1.18      8.2±0.17ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1456.8±7.27µs    11.4 MB/sec    1.13  1644.3±10.36µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.0±2.77µs    18.7 MB/sec    1.11    175.8±5.00µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.02ms     8.3 MB/sec    1.14      3.5±0.06ms     7.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.0±0.08ms     4.1 MB/sec    1.00     10.1±0.11ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1931.7±30.79µs     8.6 MB/sec    1.00  1941.1±30.39µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    214.3±5.15µs    13.8 MB/sec    1.03   221.2±12.55µs    13.3 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.09ms     6.0 MB/sec    1.01      4.2±0.04ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.01     14.3±0.15ms     2.8 MB/sec    1.00     14.1±0.11ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.02ms     4.5 MB/sec    1.00      3.7±0.04ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.01    446.6±5.96µs     6.6 MB/sec    1.00   443.4±14.29µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.11ms     4.0 MB/sec    1.00      6.4±0.12ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.06ms     5.6 MB/sec    1.00      7.2±0.06ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1473.5±19.10µs    11.3 MB/sec    1.00  1475.6±23.04µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.02    168.1±7.00µs    17.6 MB/sec    1.00    164.0±2.42µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.04ms     8.1 MB/sec    1.00      3.1±0.03ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
