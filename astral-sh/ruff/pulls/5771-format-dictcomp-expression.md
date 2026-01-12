```yaml
number: 5771
title: "Format `DictComp` expression"
type: pull_request
state: merged
author: cnpryer
labels: []
assignees: []
merged: true
base: main
head: expr-dict-comp
created_at: 2023-07-15T02:57:52Z
updated_at: 2023-07-15T16:41:34Z
url: https://github.com/astral-sh/ruff/pull/5771
synced_at: 2026-01-12T03:30:21Z
```

# Format `DictComp` expression

---

_Pull request opened by @cnpryer on 2023-07-15 02:57_

## Summary

Format `DictComp` like `ListComp` from #5600. It's not 100%, but I figured maybe it's worth starting to explore.

## Test Plan

Added ruff fixture based on `ListComp`'s.

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__expression.py.snap`:327 on 2023-07-15 02:58_

Note the generators formatting.

---

_@cnpryer reviewed on 2023-07-15 02:58_

---

_@cnpryer reviewed on 2023-07-15 03:07_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__expression.py.snap`:327 on 2023-07-15 03:07_

cc: @davidszotten 

---

_Comment by @github-actions[bot] on 2023-07-15 03:14_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.02ms     4.4 MB/sec    1.01      9.4±0.03ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1857.4±2.27µs     9.0 MB/sec    1.01   1868.2±3.25µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    207.6±1.00µs    14.2 MB/sec    1.01    209.9±0.30µs    14.1 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.01ms     6.3 MB/sec    1.00      4.1±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.14ms     3.0 MB/sec    1.00     13.7±0.08ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.01      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    449.6±0.86µs     6.6 MB/sec    1.00    451.0±1.63µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.07ms     4.2 MB/sec    1.00      6.1±0.04ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.07ms     6.1 MB/sec    1.01      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1467.1±2.43µs    11.4 MB/sec    1.01   1486.3±4.60µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.4±0.91µs    17.7 MB/sec    1.02    169.0±0.22µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.02      3.1±0.01ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     11.7±0.45ms     3.5 MB/sec    1.00     11.5±0.55ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.15ms     7.7 MB/sec    1.04      2.3±0.09ms     7.4 MB/sec
formatter/numpy/globals.py                 1.00    240.6±4.57µs    12.3 MB/sec    1.02    246.3±6.45µs    12.0 MB/sec
formatter/pydantic/types.py                1.00      4.6±0.06ms     5.5 MB/sec    1.01      4.7±0.05ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.03     16.3±0.65ms     2.5 MB/sec    1.00     15.8±0.45ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      4.3±0.21ms     3.8 MB/sec    1.00      4.1±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.04  551.2±104.81µs     5.4 MB/sec    1.00   530.2±25.05µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.28ms     3.5 MB/sec    1.02      7.5±0.30ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.32ms     4.9 MB/sec    1.00      8.2±0.27ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1744.6±70.40µs     9.5 MB/sec    1.01  1767.3±52.69µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    202.1±4.98µs    14.6 MB/sec    1.01    204.9±8.17µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.13ms     6.7 MB/sec    1.00      3.8±0.14ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Format `ExprDictComp`" to "Format `DictComp` expression" by @cnpryer on 2023-07-15 03:28_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__expression.py.snap`:327 on 2023-07-15 14:54_

You mean the parentheses around the tuple and that it expands over multiple lines? We probably need to use the `StripInsideForLoop` layout here as well (unrelated to your PR)

https://github.com/astral-sh/ruff/issues/5779

---

_@MichaReiser approved on 2023-07-15 14:58_

Nice! Thank you.

---

_Merged by @MichaReiser on 2023-07-15 16:35_

---

_Closed by @MichaReiser on 2023-07-15 16:35_

---

_Branch deleted on 2023-07-15 16:41_

---
