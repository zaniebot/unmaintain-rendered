```yaml
number: 5666
title: "Add Jupyter Notebook usage with `pre-commit` in docs"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - documentation
assignees: []
merged: true
base: main
head: dhruv/pre-commit-docs
created_at: 2023-07-10T19:09:54Z
updated_at: 2023-07-10T19:39:10Z
url: https://github.com/astral-sh/ruff/pull/5666
synced_at: 2026-01-12T15:55:19Z
```

# Add Jupyter Notebook usage with `pre-commit` in docs

---

_@dhruvmanila_

Similar to https://github.com/astral-sh/ruff-pre-commit/pull/45

---

_Review requested from @charliermarsh by @dhruvmanila on 2023-07-10 19:10_

---

_@charliermarsh approved on 2023-07-10 19:12_

---

_Merged by @dhruvmanila on 2023-07-10 19:17_

---

_Closed by @dhruvmanila on 2023-07-10 19:17_

---

_Branch deleted on 2023-07-10 19:17_

---

_Label `documentation` added by @dhruvmanila on 2023-07-10 19:17_

---

_@konstin approved on 2023-07-10 19:18_

---

_Comment by @github-actions[bot] on 2023-07-10 19:22_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.5±0.03ms     4.8 MB/sec    1.01      8.5±0.07ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.02   1841.3±3.89µs     9.0 MB/sec    1.00   1801.8±4.87µs     9.2 MB/sec
formatter/numpy/globals.py                 1.01    200.3±3.14µs    14.7 MB/sec    1.00    198.7±1.66µs    14.9 MB/sec
formatter/pydantic/types.py                1.03      4.2±0.02ms     6.1 MB/sec    1.00      4.1±0.02ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     14.3±0.05ms     2.9 MB/sec    1.00     14.3±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.7 MB/sec    1.00      3.6±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    368.4±4.27µs     8.0 MB/sec    1.01    372.8±2.43µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.03ms     4.1 MB/sec    1.01      6.4±0.03ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.03ms     5.6 MB/sec    1.01      7.3±0.03ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1486.9±24.26µs    11.2 MB/sec    1.00   1490.1±7.08µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    159.4±0.40µs    18.5 MB/sec    1.00    158.8±0.71µs    18.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     7.9 MB/sec    1.00      3.2±0.02ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.0±0.09ms     4.1 MB/sec    1.00      9.9±0.12ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.2±0.04ms     7.6 MB/sec    1.00      2.1±0.03ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00   239.3±13.65µs    12.3 MB/sec    1.01   241.4±11.78µs    12.2 MB/sec
formatter/pydantic/types.py                1.01      4.8±0.06ms     5.3 MB/sec    1.00      4.8±0.07ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.9±0.21ms     2.6 MB/sec    1.01     16.0±0.20ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.0 MB/sec    1.01      4.1±0.09ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    500.5±9.48µs     5.9 MB/sec    1.00   499.1±15.23µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.10ms     3.6 MB/sec    1.00      7.1±0.09ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.1±0.06ms     5.0 MB/sec    1.00      8.1±0.07ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1722.4±21.45µs     9.7 MB/sec    1.00  1701.8±19.43µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    198.6±3.68µs    14.9 MB/sec    1.00    195.8±3.20µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.0 MB/sec    1.00      3.6±0.04ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
