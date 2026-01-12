```yaml
number: 5926
title: "Restore `redefined-while-unused` violations in classes"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/class
created_at: 2023-07-20T15:57:13Z
updated_at: 2023-07-20T16:28:38Z
url: https://github.com/astral-sh/ruff/pull/5926
synced_at: 2026-01-12T15:55:20Z
```

# Restore `redefined-while-unused` violations in classes

---

_@charliermarsh_

## Summary

This is a regression from a recent refactor whereby we moved these checks to a deferred pass.

Closes https://github.com/astral-sh/ruff/issues/5918.



---

_Comment by @github-actions[bot] on 2023-07-20 16:08_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.0±0.05ms     3.7 MB/sec    1.03     11.3±0.08ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.01ms     7.5 MB/sec    1.02      2.3±0.01ms     7.4 MB/sec
formatter/numpy/globals.py                 1.00    243.2±2.11µs    12.1 MB/sec    1.03    249.9±2.55µs    11.8 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.03ms     5.3 MB/sec    1.02      4.9±0.03ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.00     15.5±0.09ms     2.6 MB/sec    1.00     15.6±0.11ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.02ms     4.2 MB/sec    1.01      4.0±0.01ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    522.2±3.15µs     5.7 MB/sec    1.00    519.4±2.71µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.04ms     3.6 MB/sec    1.00      7.0±0.03ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.03ms     5.2 MB/sec    1.00      7.9±0.03ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1699.9±3.69µs     9.8 MB/sec    1.00   1688.9±4.17µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    190.9±6.00µs    15.5 MB/sec    1.00    188.9±0.38µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.1 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     11.0±0.10ms     3.7 MB/sec    1.00     10.9±0.09ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.06ms     7.7 MB/sec    1.04      2.3±0.17ms     7.4 MB/sec
formatter/numpy/globals.py                 1.00    238.5±4.19µs    12.4 MB/sec    1.03    245.0±6.86µs    12.0 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.05ms     5.4 MB/sec    1.00      4.7±0.07ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.01     15.4±0.23ms     2.6 MB/sec    1.00     15.2±0.14ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.11ms     4.1 MB/sec    1.00      4.0±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    492.3±8.83µs     6.0 MB/sec    1.00    490.0±7.18µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.06ms     3.7 MB/sec    1.00      6.9±0.09ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.02      8.0±0.19ms     5.1 MB/sec    1.00      7.9±0.07ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1666.0±13.00µs    10.0 MB/sec    1.00  1658.0±17.58µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    191.8±3.22µs    15.4 MB/sec    1.00    189.4±2.25µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.03ms     7.2 MB/sec    1.00      3.5±0.05ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-20 16:10_

---

_Closed by @charliermarsh on 2023-07-20 16:10_

---

_Branch deleted on 2023-07-20 16:10_

---

_Label `bug` added by @charliermarsh on 2023-07-20 16:10_

---
