```yaml
number: 5094
title: "Re-export `ruff_python_semantic` members"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/semantic
created_at: 2023-06-14T18:13:16Z
updated_at: 2023-06-14T18:46:39Z
url: https://github.com/astral-sh/ruff/pull/5094
synced_at: 2026-01-12T03:43:30Z
```

# Re-export `ruff_python_semantic` members

---

_Pull request opened by @charliermarsh on 2023-06-14 18:13_

## Summary

This PR adds a more unified public API to `ruff_python_semantic`, so that we don't need to do deeply nested imports all over the place.

---

_Merged by @charliermarsh on 2023-06-14 18:23_

---

_Closed by @charliermarsh on 2023-06-14 18:23_

---

_Branch deleted on 2023-06-14 18:23_

---

_Comment by @github-actions[bot] on 2023-06-14 18:27_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.3±0.05ms     6.5 MB/sec    1.02      6.4±0.04ms     6.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1310.0±7.04µs    12.7 MB/sec    1.02   1333.6±4.09µs    12.5 MB/sec
formatter/numpy/globals.py                 1.00    125.2±0.23µs    23.6 MB/sec    1.01    126.1±0.17µs    23.4 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.02ms     9.9 MB/sec    1.02      2.6±0.01ms     9.8 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.03ms     3.0 MB/sec    1.00     13.6±0.07ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.00ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    419.2±1.10µs     7.0 MB/sec    1.00    418.0±0.76µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.02ms     4.4 MB/sec    1.01      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.02ms     6.3 MB/sec    1.01      6.6±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1421.8±2.53µs    11.7 MB/sec    1.00   1422.7±4.59µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.02    160.6±1.23µs    18.4 MB/sec    1.00    157.3±0.28µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.6 MB/sec    1.01      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.10ms     5.2 MB/sec    1.01      7.9±0.19ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1611.0±29.55µs    10.3 MB/sec    1.00  1616.2±26.78µs    10.3 MB/sec
formatter/numpy/globals.py                 1.00    152.9±2.98µs    19.3 MB/sec    1.04   158.9±24.31µs    18.6 MB/sec
formatter/pydantic/types.py                1.01      3.2±0.11ms     7.9 MB/sec    1.00      3.2±0.07ms     7.9 MB/sec
linter/all-rules/large/dataset.py          1.01     17.1±0.28ms     2.4 MB/sec    1.00     17.0±0.20ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.10ms     3.9 MB/sec    1.00      4.3±0.09ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01   510.9±10.91µs     5.8 MB/sec    1.00    504.8±7.86µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.12ms     3.5 MB/sec    1.01      7.4±0.17ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.09ms     4.8 MB/sec    1.00      8.5±0.09ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1779.9±23.73µs     9.4 MB/sec    1.01  1802.7±52.78µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    202.4±4.25µs    14.6 MB/sec    1.00    202.2±4.77µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.7 MB/sec    1.01      3.8±0.05ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
