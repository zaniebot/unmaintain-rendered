```yaml
number: 5283
title: Update docs to match updated logo and color palette
type: pull_request
state: merged
author: trag1c
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/docs-refresh
created_at: 2023-06-22T03:33:18Z
updated_at: 2023-06-22T15:19:35Z
url: https://github.com/astral-sh/ruff/pull/5283
synced_at: 2026-01-12T15:55:18Z
```

# Update docs to match updated logo and color palette

---

_@trag1c_

![8511](https://github.com/astral-sh/ruff/assets/77130613/862d151f-ff1d-4da8-9230-8dd32f41f197)

## Summary

Supersedes #5277, includes redesigned dark mode.

## Test Plan

* `python scripts/generate_mkdocs.py`
* `mkdocs serve`


---

_Comment by @github-actions[bot] on 2023-06-22 03:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.32ms     4.9 MB/sec    1.00      8.2±0.28ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1749.7±69.96µs     9.5 MB/sec    1.00  1744.4±73.95µs     9.5 MB/sec
formatter/numpy/globals.py                 1.00    166.8±7.81µs    17.7 MB/sec    1.05    175.8±9.88µs    16.8 MB/sec
formatter/pydantic/types.py                1.05      3.5±0.24ms     7.2 MB/sec    1.00      3.4±0.23ms     7.5 MB/sec
linter/all-rules/large/dataset.py          1.00     17.0±0.69ms     2.4 MB/sec    1.01     17.1±0.81ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.19ms     4.1 MB/sec    1.00      4.1±0.16ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   522.6±28.29µs     5.6 MB/sec    1.10   572.9±35.40µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.31ms     3.6 MB/sec    1.08      7.7±0.27ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.41ms     4.7 MB/sec    1.01      8.6±0.36ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.08  1943.4±62.84µs     8.6 MB/sec    1.00  1797.1±93.36µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   213.6±10.81µs    13.8 MB/sec    1.03   219.2±10.08µs    13.5 MB/sec
linter/default-rules/pydantic/types.py     1.07      4.2±0.47ms     6.1 MB/sec    1.00      3.9±0.22ms     6.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.5±0.35ms     3.9 MB/sec    1.04     10.9±0.53ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.2±0.15ms     7.5 MB/sec    1.00      2.2±0.13ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00   224.0±11.73µs    13.2 MB/sec    1.02   227.6±20.42µs    13.0 MB/sec
formatter/pydantic/types.py                1.03      4.6±0.24ms     5.6 MB/sec    1.00      4.4±0.20ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.05     21.3±0.83ms  1956.5 KB/sec    1.00     20.2±0.67ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      5.7±0.39ms     2.9 MB/sec    1.00      5.5±0.28ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.03   667.5±50.11µs     4.4 MB/sec    1.00   645.1±32.19µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.4±0.43ms     2.7 MB/sec    1.02      9.6±0.43ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.00     10.4±0.33ms     3.9 MB/sec    1.06     11.0±0.46ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.09ms     7.5 MB/sec    1.10      2.4±0.15ms     6.9 MB/sec
linter/default-rules/numpy/globals.py      1.01   277.0±22.20µs    10.7 MB/sec    1.00   275.5±15.86µs    10.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.18ms     5.3 MB/sec    1.10      5.3±0.45ms     4.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-06-22 15:19_

Thank you so much!

---

_Label `documentation` added by @charliermarsh on 2023-06-22 15:19_

---

_Merged by @charliermarsh on 2023-06-22 15:19_

---

_Closed by @charliermarsh on 2023-06-22 15:19_

---
