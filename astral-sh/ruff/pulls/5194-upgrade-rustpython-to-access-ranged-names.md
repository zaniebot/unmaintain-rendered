```yaml
number: 5194
title: "Upgrade `RustPython` to access ranged names"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/id
created_at: 2023-06-19T22:06:20Z
updated_at: 2023-06-20T16:05:39Z
url: https://github.com/astral-sh/ruff/pull/5194
synced_at: 2026-01-12T03:43:30Z
```

# Upgrade `RustPython` to access ranged names

---

_Pull request opened by @charliermarsh on 2023-06-19 22:06_

## Summary

In https://github.com/astral-sh/RustPython-Parser/pull/8, we modified RustPython to include ranges for any identifiers that aren't `Expr::Name` (which already has an identifier).

For example, the `e` in `except ValueError as e` was previously un-ranged. To extract its range, we had to do some lexing of our own. This change should improve performance and let us remove a bunch of code.

## Test Plan

`cargo test`


---

_Review requested from @MichaReiser by @charliermarsh on 2023-06-19 22:06_

---

_Review requested from @konstin by @charliermarsh on 2023-06-19 22:06_

---

_Comment by @github-actions[bot] on 2023-06-19 22:44_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.4±0.07ms     6.3 MB/sec    1.01      6.5±0.07ms     6.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1311.4±3.08µs    12.7 MB/sec    1.02  1332.6±23.44µs    12.5 MB/sec
formatter/numpy/globals.py                 1.00    128.2±0.16µs    23.0 MB/sec    1.02    130.3±0.20µs    22.6 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.01ms     9.8 MB/sec    1.01      2.6±0.08ms     9.7 MB/sec
linter/all-rules/large/dataset.py          1.00     13.4±0.17ms     3.0 MB/sec    1.03     13.7±0.14ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.06ms     5.0 MB/sec    1.00      3.3±0.06ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    419.5±1.32µs     7.0 MB/sec    1.03   431.5±12.90µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.09ms     4.2 MB/sec    1.01      6.1±0.08ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.11ms     6.1 MB/sec    1.02      6.8±0.07ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1436.8±4.54µs    11.6 MB/sec    1.02   1463.2±2.87µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.5±1.39µs    18.2 MB/sec    1.02    166.3±0.32µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.03ms     8.3 MB/sec    1.00      3.0±0.02ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     10.7±0.35ms     3.8 MB/sec    1.00     10.5±0.31ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.07ms     7.8 MB/sec    1.00      2.1±0.07ms     7.8 MB/sec
formatter/numpy/globals.py                 1.02   222.7±13.94µs    13.3 MB/sec    1.00   219.1±15.03µs    13.5 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.13ms     5.9 MB/sec    1.00      4.3±0.11ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     20.5±0.66ms  2036.0 KB/sec    1.04     21.2±0.73ms  1963.2 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.4±0.18ms     3.1 MB/sec    1.01      5.4±0.16ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   659.1±18.46µs     4.5 MB/sec    1.01   666.9±20.52µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.2±0.36ms     2.8 MB/sec    1.02      9.4±0.36ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.00     10.8±0.34ms     3.8 MB/sec    1.00     10.8±0.30ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.08ms     7.3 MB/sec    1.00      2.3±0.05ms     7.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   280.8±11.93µs    10.5 MB/sec    1.00   281.5±17.10µs    10.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.9±0.17ms     5.2 MB/sec    1.00      4.9±0.15ms     5.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:629 on 2023-06-20 05:25_

- I wonder if we can simplify this now by simply using the argument with default range. Also... did you override the new visit methods in the CommentVisitor?

---

_@MichaReiser reviewed on 2023-06-20 05:25_

---

_@MichaReiser reviewed on 2023-06-20 06:47_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:629 on 2023-06-20 06:47_

Fixed in https://github.com/astral-sh/ruff/pull/5204/

---

_@MichaReiser approved on 2023-06-20 06:56_

Looks good. Some other places that could use the new `Identifier` range

* Function definition formatting: https://github.com/charliermarsh/ruff/blob/84dd7db9c367e4aeeb2fbfc156a1f760787871e6/crates/ruff_python_formatter/src/statement/stmt_function_def.rs#L88
* Argument formatting https://github.com/charliermarsh/ruff/blob/84dd7db9c367e4aeeb2fbfc156a1f760787871e6/crates/ruff_python_formatter/src/other/arg.rs#L23

---

_Label `internal` added by @MichaReiser on 2023-06-20 06:56_

---

_@charliermarsh reviewed on 2023-06-20 15:29_

---

_Review comment by @charliermarsh on `Cargo.toml`:52 on 2023-06-20 15:29_

FYI @MichaReiser - I'm going to start documenting these here so that we don't forget to update the tags.

---

_Merged by @charliermarsh on 2023-06-20 15:43_

---

_Closed by @charliermarsh on 2023-06-20 15:43_

---

_Branch deleted on 2023-06-20 15:43_

---
