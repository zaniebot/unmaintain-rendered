```yaml
number: 5223
title: "Complete `flake8-debugger` documentation"
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: debugger-docs
created_at: 2023-06-20T18:47:29Z
updated_at: 2023-07-10T09:55:12Z
url: https://github.com/astral-sh/ruff/pull/5223
synced_at: 2026-01-12T03:36:54Z
```

# Complete `flake8-debugger` documentation

---

_Pull request opened by @tjkuson on 2023-06-20 18:47_

## Summary

Completes the documentation for the `flake8-debugger` ruleset. Related to #2646.

## Test Plan

`python scripts/check_docs_formatted.py`

---

_Renamed from "Debugger docs" to "Complete `flake8-debugger` documentation" by @tjkuson on 2023-06-20 18:47_

---

_Comment by @github-actions[bot] on 2023-06-20 19:01_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.4±0.11ms     5.5 MB/sec    1.03      7.6±0.05ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1555.6±28.27µs    10.7 MB/sec    1.01  1573.3±11.01µs    10.6 MB/sec
formatter/numpy/globals.py                 1.00    154.9±3.44µs    19.1 MB/sec    1.03   159.1±12.99µs    18.5 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.06ms     8.3 MB/sec    1.02      3.1±0.02ms     8.1 MB/sec
linter/all-rules/large/dataset.py          1.01     15.5±0.29ms     2.6 MB/sec    1.00     15.4±0.21ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.9±0.03ms     4.2 MB/sec    1.00      3.9±0.07ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    494.9±9.75µs     6.0 MB/sec    1.04    514.2±6.01µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.10ms     3.8 MB/sec    1.01      6.9±0.10ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      7.8±0.12ms     5.2 MB/sec    1.00      7.7±0.12ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1704.5±30.42µs     9.8 MB/sec    1.00  1701.4±23.41µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.7±4.40µs    15.0 MB/sec    1.06    208.9±7.27µs    14.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.05ms     7.1 MB/sec    1.02      3.6±0.03ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.1±0.46ms     4.0 MB/sec    1.04     10.5±0.41ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.11ms     8.1 MB/sec    1.05      2.2±0.10ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00   215.6±12.77µs    13.7 MB/sec    1.06   229.1±24.52µs    12.9 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.16ms     5.9 MB/sec    1.03      4.4±0.20ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.00     21.0±0.83ms  1984.2 KB/sec    1.00     21.0±0.79ms  1982.1 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      5.7±0.25ms     2.9 MB/sec    1.00      5.5±0.22ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   663.8±38.26µs     4.4 MB/sec    1.01   667.5±34.35µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.5±0.37ms     2.7 MB/sec    1.00      9.5±0.50ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.01     11.2±0.43ms     3.6 MB/sec    1.00     11.1±0.46ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.11ms     7.4 MB/sec    1.01      2.3±0.08ms     7.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   269.3±14.87µs    11.0 MB/sec    1.02   275.0±11.61µs    10.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.9±0.24ms     5.2 MB/sec    1.01      4.9±0.26ms     5.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-06-20 20:53_

---

_Merged by @charliermarsh on 2023-06-20 21:04_

---

_Closed by @charliermarsh on 2023-06-20 21:04_

---

_Branch deleted on 2023-07-10 09:55_

---
