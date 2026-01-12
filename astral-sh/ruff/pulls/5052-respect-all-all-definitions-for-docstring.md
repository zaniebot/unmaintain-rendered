```yaml
number: 5052
title: "Respect all `__all__` definitions for docstring visibility"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/all
created_at: 2023-06-13T16:03:39Z
updated_at: 2023-06-13T16:35:23Z
url: https://github.com/astral-sh/ruff/pull/5052
synced_at: 2026-01-12T15:55:17Z
```

# Respect all `__all__` definitions for docstring visibility

---

_@charliermarsh_

## Summary

We changed the semantics around `__all__` in #4885, but didn't update the docstring visibility code to match those changes.

---

_Comment by @github-actions[bot] on 2023-06-13 16:14_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.5±0.03ms     5.4 MB/sec    1.00      7.6±0.05ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1570.1±2.24µs    10.6 MB/sec    1.00   1569.2±5.27µs    10.6 MB/sec
formatter/numpy/globals.py                 1.00    149.8±0.64µs    19.7 MB/sec    1.00    150.2±1.21µs    19.6 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.02ms     8.2 MB/sec
linter/all-rules/large/dataset.py          1.00     16.6±0.11ms     2.4 MB/sec    1.02     17.0±0.13ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.01ms     4.2 MB/sec    1.02      4.1±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    500.1±5.39µs     5.9 MB/sec    1.00    499.8±2.33µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.07ms     3.6 MB/sec    1.01      7.2±0.08ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.05ms     5.0 MB/sec    1.02      8.3±0.05ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1747.9±7.65µs     9.5 MB/sec    1.01   1762.3±7.97µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.6±2.65µs    14.9 MB/sec    1.00    197.2±0.87µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     7.0 MB/sec    1.01      3.7±0.02ms     6.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.23     10.9±0.46ms     3.7 MB/sec    1.00      8.8±0.20ms     4.6 MB/sec
formatter/numpy/ctypeslib.py               1.17      2.1±0.09ms     7.8 MB/sec    1.00  1816.4±40.65µs     9.2 MB/sec
formatter/numpy/globals.py                 1.11   205.3±12.53µs    14.4 MB/sec    1.00    185.0±8.52µs    16.0 MB/sec
formatter/pydantic/types.py                1.17      4.3±0.14ms     5.9 MB/sec    1.00      3.7±0.11ms     6.9 MB/sec
linter/all-rules/large/dataset.py          1.00     18.4±0.29ms     2.2 MB/sec    1.02     18.8±0.37ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.14ms     3.5 MB/sec    1.02      4.9±0.16ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   574.1±17.86µs     5.1 MB/sec    1.01   577.9±20.88µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.1±0.20ms     3.1 MB/sec    1.02      8.3±0.21ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.6±0.22ms     4.2 MB/sec    1.00      9.6±0.22ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.08ms     8.2 MB/sec    1.00      2.0±0.09ms     8.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    236.9±9.02µs    12.5 MB/sec    1.00    237.5±6.42µs    12.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.12ms     5.9 MB/sec    1.00      4.3±0.10ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-13 16:22_

---

_Closed by @charliermarsh on 2023-06-13 16:22_

---

_Branch deleted on 2023-06-13 16:22_

---
