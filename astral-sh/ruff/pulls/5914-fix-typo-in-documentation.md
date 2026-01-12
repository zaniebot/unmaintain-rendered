```yaml
number: 5914
title: Fix typo in documentation
type: pull_request
state: merged
author: tjkuson
labels: []
assignees: []
merged: true
base: main
head: fixme-typo
created_at: 2023-07-20T10:49:23Z
updated_at: 2023-07-20T11:17:44Z
url: https://github.com/astral-sh/ruff/pull/5914
synced_at: 2026-01-12T15:55:19Z
```

# Fix typo in documentation

---

_@tjkuson_

## Summary

Fixes typo in #5868.

## Test Plan

`python scripts/check_docs_formatted.py`


---

_Comment by @github-actions[bot] on 2023-07-20 10:58_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.5±0.35ms     4.3 MB/sec    1.03      9.8±0.39ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1865.1±68.45µs     8.9 MB/sec    1.03  1919.7±93.55µs     8.7 MB/sec
formatter/numpy/globals.py                 1.07   223.9±13.38µs    13.2 MB/sec    1.00   209.7±10.26µs    14.1 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.16ms     6.3 MB/sec    1.04      4.2±0.17ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.03     14.8±0.43ms     2.7 MB/sec    1.00     14.4±1.14ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.18ms     4.7 MB/sec    1.00      3.5±0.14ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   450.7±15.24µs     6.5 MB/sec    1.01   455.8±19.51µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.07      6.5±0.28ms     3.9 MB/sec    1.00      6.1±0.24ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.25ms     5.7 MB/sec    1.00      7.2±0.29ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1573.2±57.20µs    10.6 MB/sec    1.00  1573.2±75.27µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.05    169.1±7.03µs    17.5 MB/sec    1.00    161.4±6.70µs    18.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.12ms     8.1 MB/sec    1.04      3.3±0.11ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.4±1.13ms     3.6 MB/sec    1.00     11.4±0.54ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.17ms     7.5 MB/sec    1.02      2.3±0.15ms     7.4 MB/sec
formatter/numpy/globals.py                 1.00   249.7±16.88µs    11.8 MB/sec    1.02   255.6±28.00µs    11.5 MB/sec
formatter/pydantic/types.py                1.00      4.9±0.21ms     5.3 MB/sec    1.03      5.0±0.36ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.00     16.0±0.80ms     2.5 MB/sec    1.00     15.9±0.60ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.2±0.21ms     4.0 MB/sec    1.00      4.1±0.15ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   501.6±53.82µs     5.9 MB/sec    1.02   510.4±45.92µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.68ms     3.6 MB/sec    1.02      7.3±0.81ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.51ms     4.9 MB/sec    1.00      8.3±0.34ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1718.3±73.42µs     9.7 MB/sec    1.02  1746.3±104.64µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.02   199.5±16.68µs    14.8 MB/sec    1.00   196.1±13.05µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.19ms     6.9 MB/sec    1.07      4.0±0.73ms     6.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-07-20 11:06_

Thank you

---

_Merged by @MichaReiser on 2023-07-20 11:06_

---

_Closed by @MichaReiser on 2023-07-20 11:06_

---

_Branch deleted on 2023-07-20 11:09_

---
