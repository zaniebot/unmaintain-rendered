```yaml
number: 5689
title: "Format `ModExpression`"
type: pull_request
state: merged
author: konstin
labels:
  - formatter
assignees: []
merged: true
base: main
head: format_mod_expression
created_at: 2023-07-11T13:57:40Z
updated_at: 2023-07-11T14:41:11Z
url: https://github.com/astral-sh/ruff/pull/5689
synced_at: 2026-01-12T15:55:19Z
```

# Format `ModExpression`

---

_@konstin_

## Summary

We don't use `ModExpression` anywhere but it's part of the AST, removes one `not_implemented_yet` and is a trivial 2-liner, so i implemented formatting for `ModExpression`.

## Test Plan

None, this kind of node does not occur in file input. Otherwise all the tests for expressions

---

_Review requested from @MichaReiser by @konstin on 2023-07-11 13:57_

---

_Label `formatter` added by @konstin on 2023-07-11 13:57_

---

_Renamed from "Format ModExpression" to "Format `ModExpression`" by @konstin on 2023-07-11 13:58_

---

_Comment by @github-actions[bot] on 2023-07-11 14:09_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.6±0.49ms     4.3 MB/sec    1.05     10.0±0.38ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.13ms     7.8 MB/sec    1.08      2.3±0.10ms     7.2 MB/sec
formatter/numpy/globals.py                 1.00   246.0±26.45µs    12.0 MB/sec    1.10   270.3±12.00µs    10.9 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.26ms     5.3 MB/sec    1.03      4.9±0.26ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.00     16.4±0.60ms     2.5 MB/sec    1.12     18.3±1.07ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.34ms     4.1 MB/sec    1.06      4.4±0.42ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   524.0±33.46µs     5.6 MB/sec    1.08   565.9±28.43µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.36ms     3.4 MB/sec    1.08      8.0±0.22ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.37ms     4.9 MB/sec    1.17      9.7±0.25ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1701.5±79.93µs     9.8 MB/sec    1.19      2.0±0.07ms     8.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   210.5±13.38µs    14.0 MB/sec    1.10   232.2±10.37µs    12.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.14ms     6.8 MB/sec    1.16      4.3±0.14ms     5.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.5±0.12ms     4.3 MB/sec    1.01      9.7±0.13ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.03ms     7.7 MB/sec    1.01      2.2±0.04ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00    239.8±4.00µs    12.3 MB/sec    1.03    245.9±9.51µs    12.0 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.07ms     5.5 MB/sec    1.02      4.8±0.08ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     16.1±0.20ms     2.5 MB/sec    1.02     16.5±0.27ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.06ms     4.0 MB/sec    1.02      4.3±0.07ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    512.4±8.85µs     5.8 MB/sec    1.01   520.0±11.63µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.13ms     3.5 MB/sec    1.03      7.6±0.20ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.10ms     4.9 MB/sec    1.01      8.3±0.12ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1742.3±22.31µs     9.6 MB/sec    1.01  1761.6±27.95µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.3±3.29µs    14.5 MB/sec    1.00    204.2±6.39µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.9 MB/sec    1.00      3.7±0.05ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-07-11 14:39_

---

_Merged by @konstin on 2023-07-11 14:41_

---

_Closed by @konstin on 2023-07-11 14:41_

---

_Branch deleted on 2023-07-11 14:41_

---
