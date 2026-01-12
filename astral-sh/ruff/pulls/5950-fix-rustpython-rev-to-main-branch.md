```yaml
number: 5950
title: Fix RustPython rev to main branch
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: fix_parser_ref
created_at: 2023-07-21T15:44:32Z
updated_at: 2023-07-21T16:11:33Z
url: https://github.com/astral-sh/ruff/pull/5950
synced_at: 2026-01-12T03:30:22Z
```

# Fix RustPython rev to main branch

---

_Pull request opened by @konstin on 2023-07-21 15:44_

**Summary** I accidentally merged earlier while the RustPython parser rev was still pointing to the feature branch instead of to the merged main. This make the rev point to the RustPython parser repo main again

---

_Label `internal` added by @konstin on 2023-07-21 15:44_

---

_Merged by @konstin on 2023-07-21 15:53_

---

_Closed by @konstin on 2023-07-21 15:53_

---

_Branch deleted on 2023-07-21 15:53_

---

_Comment by @github-actions[bot] on 2023-07-21 15:57_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.52ms     3.7 MB/sec    1.00     10.9±0.35ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.09ms     7.8 MB/sec    1.05      2.3±0.14ms     7.4 MB/sec
formatter/numpy/globals.py                 1.02   253.9±19.10µs    11.6 MB/sec    1.00   249.8±10.22µs    11.8 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.21ms     5.4 MB/sec    1.01      4.8±0.25ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.5±0.55ms     2.6 MB/sec    1.02     15.9±0.53ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.20ms     4.3 MB/sec    1.05      4.1±0.23ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   534.9±17.29µs     5.5 MB/sec    1.00   534.8±21.02µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.35ms     3.6 MB/sec    1.03      7.3±0.28ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      7.9±0.34ms     5.2 MB/sec    1.00      7.8±0.41ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1678.6±73.32µs     9.9 MB/sec    1.00  1674.6±159.20µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.08    204.7±7.45µs    14.4 MB/sec    1.00   190.1±10.00µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.20ms     7.0 MB/sec    1.00      3.6±0.27ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     14.4±0.81ms     2.8 MB/sec    1.19     17.1±0.89ms     2.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.8±0.23ms     5.9 MB/sec    1.15      3.2±0.18ms     5.1 MB/sec
formatter/numpy/globals.py                 1.00   317.2±20.53µs     9.3 MB/sec    1.10   350.2±31.28µs     8.4 MB/sec
formatter/pydantic/types.py                1.00      6.1±0.35ms     4.2 MB/sec    1.14      7.0±0.31ms     3.6 MB/sec
linter/all-rules/large/dataset.py          1.00     20.1±0.87ms     2.0 MB/sec    1.01     20.3±0.81ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.3±0.25ms     3.1 MB/sec    1.00      5.3±0.22ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   639.1±34.14µs     4.6 MB/sec    1.02   651.7±31.18µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.1±0.33ms     2.8 MB/sec    1.01      9.2±0.40ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     11.0±0.54ms     3.7 MB/sec    1.04     11.5±0.99ms     3.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.08ms     7.4 MB/sec    1.03      2.3±0.08ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.02   264.5±12.10µs    11.2 MB/sec    1.00   259.6±13.11µs    11.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.20ms     5.4 MB/sec    1.02      4.8±0.19ms     5.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
