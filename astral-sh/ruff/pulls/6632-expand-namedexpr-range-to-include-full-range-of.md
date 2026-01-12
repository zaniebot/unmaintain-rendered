```yaml
number: 6632
title: "Expand `NamedExpr` range to include full range of parenthesized value"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - internal
assignees: []
merged: true
base: main
head: charlie/named-expr
created_at: 2023-08-16T22:16:07Z
updated_at: 2023-08-17T14:48:56Z
url: https://github.com/astral-sh/ruff/pull/6632
synced_at: 2026-01-12T15:55:22Z
```

# Expand `NamedExpr` range to include full range of parenthesized value

---

_@charliermarsh_

## Summary

Given:

```python
if (
    x
    :=
    (  # 4
        y # 5
    )  # 6
):
    pass
```

It turns out the parser ended the range of the `NamedExpr` at the end of `y`, rather than the end of the parenthesis that encloses `y`. This just seems like a bug -- the range should be from the start of the name on the left, to the end of the parenthesized node on the right.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2023-08-16 22:16_

---

_Label `internal` added by @charliermarsh on 2023-08-16 22:16_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-16 22:16_

---

_Review requested from @zanieb by @charliermarsh on 2023-08-16 22:16_

---

_Marked ready for review by @charliermarsh on 2023-08-16 22:16_

---

_@charliermarsh reviewed on 2023-08-16 22:18_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/python.lalrpop`:1345 on 2023-08-16 22:18_

In the example from the summary, `value.end()` is the end of `y`, not the end of its parenthesized range.

---

_Comment by @github-actions[bot] on 2023-08-16 22:29_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      3.8±0.06ms    10.7 MB/sec    1.00      3.8±0.07ms    10.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   799.1±12.03µs    20.8 MB/sec    1.00   802.3±15.57µs    20.8 MB/sec
formatter/numpy/globals.py                 1.00     82.7±1.60µs    35.7 MB/sec    1.01     83.2±1.56µs    35.5 MB/sec
formatter/pydantic/types.py                1.00  1545.9±32.90µs    16.5 MB/sec    1.02  1572.2±23.57µs    16.2 MB/sec
linter/all-rules/large/dataset.py          1.05     12.5±0.17ms     3.3 MB/sec    1.00     11.9±0.16ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      3.3±0.05ms     5.1 MB/sec    1.00      3.2±0.05ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    456.8±9.42µs     6.5 MB/sec    1.00    455.4±7.01µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.3±0.09ms     4.0 MB/sec    1.00      6.2±0.07ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.07      6.8±0.10ms     6.0 MB/sec    1.00      6.3±0.07ms     6.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05  1465.1±24.52µs    11.4 MB/sec    1.00  1400.7±22.15µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.03    170.1±2.19µs    17.4 MB/sec    1.00    165.1±2.60µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.05      3.0±0.05ms     8.5 MB/sec    1.00      2.8±0.08ms     9.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.8±0.06ms    10.7 MB/sec    1.01      3.8±0.06ms    10.6 MB/sec
formatter/numpy/ctypeslib.py               1.00   774.7±12.53µs    21.5 MB/sec    1.02   787.6±15.11µs    21.1 MB/sec
formatter/numpy/globals.py                 1.00     81.0±1.29µs    36.4 MB/sec    1.02     82.6±1.82µs    35.7 MB/sec
formatter/pydantic/types.py                1.00  1548.1±26.86µs    16.5 MB/sec    1.02  1579.0±30.32µs    16.2 MB/sec
linter/all-rules/large/dataset.py          1.00     13.3±0.18ms     3.1 MB/sec    1.00     13.3±0.26ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.03ms     4.7 MB/sec    1.01      3.6±0.04ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    447.6±5.19µs     6.6 MB/sec    1.02    455.6±8.68µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.14ms     3.7 MB/sec    1.01      6.9±0.16ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.12ms     5.6 MB/sec    1.01      7.4±0.09ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1541.1±29.51µs    10.8 MB/sec    1.01  1557.6±18.90µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.6±4.26µs    16.3 MB/sec    1.01    182.5±4.07µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.05ms     7.8 MB/sec    1.01      3.3±0.05ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/python.rs`:1 on 2023-08-17 05:44_

Why change 18k lines??? 

---

_@MichaReiser approved on 2023-08-17 05:44_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/python.rs`:1 on 2023-08-17 14:09_

Oh maybe I have the wrong version again? Or maybe I committed at some point with the wrong version?

---

_@charliermarsh reviewed on 2023-08-17 14:09_

---

_@charliermarsh reviewed on 2023-08-17 14:26_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/python.rs`:1 on 2023-08-17 14:26_

I re-ran locally on latest but still see huge changes, we must've committed an old version in the past or something.

---

_@charliermarsh reviewed on 2023-08-17 14:27_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/python.rs`:1 on 2023-08-17 14:27_

Or, actually, I guess everything just got offset:

<img width="1792" alt="Screen Shot 2023-08-17 at 10 26 53 AM" src="https://github.com/astral-sh/ruff/assets/1309177/54c55e6d-89a1-4341-995d-f0054d446458">


---

_Merged by @charliermarsh on 2023-08-17 14:34_

---

_Closed by @charliermarsh on 2023-08-17 14:34_

---

_Branch deleted on 2023-08-17 14:34_

---
