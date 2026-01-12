```yaml
number: 6403
title: "Move `empty_parenthesized` into the `parentheses.rs`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/empty-parenthesized
created_at: 2023-08-07T19:16:50Z
updated_at: 2023-08-08T19:37:24Z
url: https://github.com/astral-sh/ruff/pull/6403
synced_at: 2026-01-12T15:55:21Z
```

# Move `empty_parenthesized` into the `parentheses.rs`

---

_@charliermarsh_

## Summary

This PR moves `empty_parenthesized` such that it's peer to `parenthesized`, and changes the API to better match that of `parenthesized` (takes `&str` rather than `StaticText`, has a `with_dangling_comments` method, etc.).

It may be intentionally _not_ part of `parentheses.rs`, but to me they're so similar that it makes more sense for them to be in the same module, with the same API, etc.


---

_Review requested from @konstin by @charliermarsh on 2023-08-07 19:16_

---

_Label `formatter` added by @charliermarsh on 2023-08-07 19:16_

---

_Comment by @github-actions[bot] on 2023-08-07 19:56_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.4±0.58ms     4.8 MB/sec    1.01      8.5±0.47ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1618.4±76.61µs    10.3 MB/sec    1.02  1645.1±109.61µs    10.1 MB/sec
formatter/numpy/globals.py                 1.01   188.7±10.71µs    15.6 MB/sec    1.00   187.4±13.11µs    15.7 MB/sec
formatter/pydantic/types.py                1.05      3.6±0.17ms     7.1 MB/sec    1.00      3.4±0.15ms     7.5 MB/sec
linter/all-rules/large/dataset.py          1.04     11.5±0.71ms     3.5 MB/sec    1.00     11.0±0.54ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.14ms     5.7 MB/sec    1.01      3.0±0.17ms     5.6 MB/sec
linter/all-rules/numpy/globals.py          1.03   434.2±26.91µs     6.8 MB/sec    1.00   423.5±21.32µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.2±0.27ms     4.9 MB/sec    1.00      5.2±0.25ms     4.9 MB/sec
linter/default-rules/large/dataset.py      1.02      5.8±0.20ms     7.0 MB/sec    1.00      5.7±0.24ms     7.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1236.2±57.01µs    13.5 MB/sec    1.03  1276.3±73.79µs    13.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    140.4±6.00µs    21.0 MB/sec    1.06   148.8±10.55µs    19.8 MB/sec
linter/default-rules/pydantic/types.py     1.02      2.6±0.11ms     9.8 MB/sec    1.00      2.5±0.11ms    10.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.5±0.33ms     3.5 MB/sec    1.04     12.0±0.40ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.10ms     7.5 MB/sec    1.02      2.3±0.07ms     7.4 MB/sec
formatter/numpy/globals.py                 1.00   252.3±11.11µs    11.7 MB/sec    1.02   257.2±16.80µs    11.5 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.23ms     5.3 MB/sec    1.01      4.9±0.12ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.00     15.0±0.58ms     2.7 MB/sec    1.04     15.6±0.40ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.15ms     4.0 MB/sec    1.02      4.2±0.13ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   516.0±20.31µs     5.7 MB/sec    1.00   517.4±18.63µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.20ms     3.7 MB/sec    1.04      7.1±0.28ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.28ms     4.9 MB/sec    1.00      8.2±0.31ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1659.2±53.64µs    10.0 MB/sec    1.03  1706.5±46.54µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   210.4±10.61µs    14.0 MB/sec    1.01    211.5±9.68µs    13.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.09ms     7.1 MB/sec    1.02      3.7±0.09ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_dict.rs`:74 on 2023-08-08 06:57_

It seems all call paths have dangling comments. I would change the API to `empty_parenthesized("{", dangling, "}")` as @konstin suggested if this is the case.

---

_@MichaReiser approved on 2023-08-08 06:57_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-08 06:57_

---

_@konstin approved on 2023-08-08 10:16_

---

_@charliermarsh reviewed on 2023-08-08 19:04_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/expr_dict.rs`:74 on 2023-08-08 19:04_

Hmm, ok, I will revert if you both agree. I find it weird that it has a totally different API than `parenthesized`.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/expr_dict.rs`:74 on 2023-08-08 19:09_

(I see the value in it though, now they're (appropriately) required.)

---

_@charliermarsh reviewed on 2023-08-08 19:09_

---

_Merged by @charliermarsh on 2023-08-08 19:17_

---

_Closed by @charliermarsh on 2023-08-08 19:17_

---

_Branch deleted on 2023-08-08 19:17_

---
