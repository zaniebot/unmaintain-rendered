```yaml
number: 6709
title: Use a single node hierarchy to track statements and expressions
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/any-node-ref
created_at: 2023-08-21T00:26:41Z
updated_at: 2023-08-22T01:32:59Z
url: https://github.com/astral-sh/ruff/pull/6709
synced_at: 2026-01-12T02:52:04Z
```

# Use a single node hierarchy to track statements and expressions

---

_Pull request opened by @charliermarsh on 2023-08-21 00:26_

## Summary

This PR is a follow-up to the suggestion in https://github.com/astral-sh/ruff/pull/6345#discussion_r1285470953 to use a single stack to store all statements and expressions, rather than using separate vectors for each, which gives us something closer to a full-fidelity chain. (We can then generalize this concept to include all other AST nodes too.)

This is in part made possible by the removal of the hash map from `&Stmt` to `StatementId` (#6694), which makes it much cheaper to store these using a single interface (since doing so no longer introduces the requirement that we hash all expressions).

I'll follow-up with some profiling, but a few notes on how the data requirements have changed:

- We now store a `BranchId` for every expression, not just every statement, so that's an extra `u32`.
- We now store a single `NodeId` on every snapshot, rather than separate `StatementId` and `ExpressionId` IDs, so that's one fewer `u32` for each snapshot.
- We're probably doing a few more lookups in general, since any calls to `current_statement()` etc. now have to iterate up the node hierarchy until they identify the first statement.

## Test Plan

`cargo test`


---

_Label `internal` added by @charliermarsh on 2023-08-21 00:26_

---

_@charliermarsh reviewed on 2023-08-21 00:28_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/nodes.rs`:87 on 2023-08-21 00:28_

@MichaReiser - I really want this API to be capable of returning `&Stmt` and `&Expr` so that we can separate this refactor (and some follow-up refactors) from sweeping changes to the Ruff internals to use `ExpressionRef` etc. everywhere, which we should probably do but don't need to be coupled to this change. I can't figure out how to do this with `AnyNodeRef` -- is it possible?

---

_Comment by @github-actions[bot] on 2023-08-21 00:57_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.3±0.02ms    12.4 MB/sec    1.00      3.3±0.01ms    12.3 MB/sec
formatter/numpy/ctypeslib.py               1.00    688.9±2.27µs    24.2 MB/sec    1.00    688.4±2.32µs    24.2 MB/sec
formatter/numpy/globals.py                 1.00     72.5±0.92µs    40.7 MB/sec    1.00     72.5±0.33µs    40.7 MB/sec
formatter/pydantic/types.py                1.01  1347.9±24.92µs    18.9 MB/sec    1.00  1339.3±26.86µs    19.0 MB/sec
linter/all-rules/large/dataset.py          1.00     10.2±0.07ms     4.0 MB/sec    1.00     10.1±0.06ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.01ms     6.1 MB/sec    1.00      2.7±0.01ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    389.1±1.67µs     7.6 MB/sec    1.00    389.6±3.16µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.4±0.09ms     4.8 MB/sec    1.00      5.3±0.06ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.00      5.4±0.02ms     7.5 MB/sec    1.00      5.4±0.05ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1204.0±8.15µs    13.8 MB/sec    1.00  1203.2±17.70µs    13.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    144.6±6.28µs    20.4 MB/sec    1.03    148.4±6.35µs    19.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.02ms    10.4 MB/sec    1.00      2.5±0.04ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.01      4.8±0.31ms     8.6 MB/sec     1.00      4.7±0.27ms     8.6 MB/sec
formatter/numpy/ctypeslib.py               1.00   956.0±56.69µs    17.4 MB/sec     1.01   969.4±54.77µs    17.2 MB/sec
formatter/numpy/globals.py                 1.00     96.2±5.38µs    30.7 MB/sec     1.01     97.6±7.73µs    30.2 MB/sec
formatter/pydantic/types.py                1.00  1940.9±103.12µs    13.1 MB/sec    1.00  1948.3±103.38µs    13.1 MB/sec
linter/all-rules/large/dataset.py          1.00     17.5±0.64ms     2.3 MB/sec     1.00     17.5±0.57ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.9±0.21ms     3.4 MB/sec     1.00      4.8±0.21ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.08   655.0±49.88µs     4.5 MB/sec     1.00   606.7±36.41µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      9.3±0.49ms     2.7 MB/sec     1.00      9.2±0.33ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.02     10.0±0.48ms     4.1 MB/sec     1.00      9.8±0.54ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.08ms     8.2 MB/sec     1.01      2.1±0.14ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.02   257.2±13.71µs    11.5 MB/sec     1.00   253.1±17.38µs    11.7 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.4±0.22ms     5.8 MB/sec     1.00      4.3±0.21ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-08-21 01:27_

No significant changes before / after:

```text
group                                      any-node-ref                           main
-----                                      ------------                           ----
linter/all-rules/large/dataset.py          1.00      5.0±0.04ms     8.1 MB/sec    1.00      5.0±0.03ms     8.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01  1278.4±10.95µs    13.0 MB/sec    1.00  1265.8±18.80µs    13.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    122.5±1.57µs    24.1 MB/sec    1.00    122.2±1.31µs    24.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      2.5±0.03ms    10.1 MB/sec    1.01      2.5±0.05ms    10.0 MB/sec
linter/default-rules/large/dataset.py      1.01      3.0±0.01ms    13.7 MB/sec    1.00      2.9±0.01ms    13.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00    583.3±4.12µs    28.5 MB/sec    1.00    582.6±2.12µs    28.6 MB/sec
linter/default-rules/numpy/globals.py      1.00     60.8±0.66µs    48.5 MB/sec    1.00     60.8±0.32µs    48.5 MB/sec
linter/default-rules/pydantic/types.py     1.01   1258.7±7.00µs    20.3 MB/sec    1.00   1252.2±6.67µs    20.4 MB/sec
```

---

_Comment by @charliermarsh on 2023-08-21 03:04_

Hyperfine benchmarking shows _maybe_ a small decrease in performance (one or two milliseconds on a ~208ms baseline) but candidly it's hard to tell what's noise and what's real.

---

_@MichaReiser reviewed on 2023-08-21 07:04_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/nodes.rs`:87 on 2023-08-21 07:04_

This, unfortunately, isn't possible (and the reason why `ExpressionRef` exists). It is impossible to go from e.g. a `&StmtIf` to `&Stmt::If`

---

_@charliermarsh reviewed on 2023-08-21 12:54_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/nodes.rs`:87 on 2023-08-21 12:54_

Can you expand on why / how this motivates `ExpressionRef`? My understanding is that `ExpressionRef` has the same problem -- you can't go from `ExpressionRef::BoolOp` to `&Expr`, only `&ExprBoolOp`.

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/nodes.rs`:87 on 2023-08-21 14:52_

It's correct that you can't go from `ExpressionRef` to `&Expr`. But `ExpressionRef` allows you to go from `AnyNodeRef` to `ExprRef`. Meaning, `ExpressionRef` gives you a union that is limited to the variants of `Expr`, but can be converted to from any node.

---

_@MichaReiser reviewed on 2023-08-21 14:52_

---

_@charliermarsh reviewed on 2023-08-21 14:54_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/nodes.rs`:87 on 2023-08-21 14:54_

Okay this makes sense, but it still has the same problem, which is that if I use `AnyNodeRef` here, I have to make extensive changes to the rest of the codebase to use `StatementRef`, `ExpressionRef`, etc. instead of `&Stmt`, `&Expr`, etc.

---

_@MichaReiser approved on 2023-08-21 14:55_

---

_Merged by @charliermarsh on 2023-08-22 01:32_

---

_Closed by @charliermarsh on 2023-08-22 01:32_

---

_Branch deleted on 2023-08-22 01:32_

---
