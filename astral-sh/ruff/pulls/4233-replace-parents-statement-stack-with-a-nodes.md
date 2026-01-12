```yaml
number: 4233
title: "Replace `parents` statement stack with a `Nodes` abstraction"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/node-tree
created_at: 2023-05-05T00:53:28Z
updated_at: 2023-05-06T16:41:01Z
url: https://github.com/astral-sh/ruff/pull/4233
synced_at: 2026-01-12T15:55:15Z
```

# Replace `parents` statement stack with a `Nodes` abstraction

---

_@charliermarsh_

## Summary

Similar to #4138, this PR refactors our tracking of the "stack of current statements" to use a `Nodes` abstraction, whereby we track the ID of the current node, and every node stores the ID of its parent. This lets us avoid a bunch of clones (namely, cloning `self.ctx.parents` whenever we need to defer a context) and move some complexity behind a more well-defined API (e.g., `self.ctx.depths`, `self.ctx.child_to_parent`).

## Test Plan

Automated tests.

In the micro-benchmarks, performance improved by a few percentage points:

```
linter/default-rules/numpy/globals.py
                        time:   [72.388 µs 72.543 µs 72.705 µs]
                        thrpt:  [40.584 MiB/s 40.675 MiB/s 40.762 MiB/s]
                 change:
                        time:   [-1.1189% -0.8630% -0.6114%] (p = 0.00 < 0.05)
                        thrpt:  [+0.6151% +0.8705% +1.1316%]
                        Change within noise threshold.
Found 2 outliers among 100 measurements (2.00%)
  2 (2.00%) high mild
linter/default-rules/pydantic/types.py
                        time:   [1.4599 ms 1.4623 ms 1.4650 ms]
                        thrpt:  [17.408 MiB/s 17.440 MiB/s 17.469 MiB/s]
                 change:
                        time:   [-2.1689% -1.9893% -1.8088%] (p = 0.00 < 0.05)
                        thrpt:  [+1.8421% +2.0296% +2.2170%]
                        Performance has improved.
linter/default-rules/numpy/ctypeslib.py
                        time:   [674.25 µs 675.15 µs 676.15 µs]
                        thrpt:  [24.626 MiB/s 24.663 MiB/s 24.696 MiB/s]
                 change:
                        time:   [-1.7584% -1.5019% -1.2492%] (p = 0.00 < 0.05)
                        thrpt:  [+1.2650% +1.5248% +1.7899%]
                        Performance has improved.
Found 1 outliers among 100 measurements (1.00%)
  1 (1.00%) high severe
linter/default-rules/large/dataset.py
                        time:   [3.3299 ms 3.3340 ms 3.3385 ms]
                        thrpt:  [12.186 MiB/s 12.202 MiB/s 12.218 MiB/s]
                 change:
                        time:   [-2.5060% -2.2051% -1.9172%] (p = 0.00 < 0.05)
                        thrpt:  [+1.9547% +2.2549% +2.5704%]
                        Performance has improved.

linter/all-rules/numpy/globals.py
                        time:   [143.50 µs 143.60 µs 143.73 µs]
                        thrpt:  [20.529 MiB/s 20.547 MiB/s 20.562 MiB/s]
                 change:
                        time:   [-1.7304% -1.3775% -0.9696%] (p = 0.00 < 0.05)
                        thrpt:  [+0.9791% +1.3967% +1.7608%]
                        Change within noise threshold.
Found 7 outliers among 100 measurements (7.00%)
  4 (4.00%) high mild
  3 (3.00%) high severe
linter/all-rules/pydantic/types.py
                        time:   [2.8682 ms 2.8702 ms 2.8722 ms]
                        thrpt:  [8.8792 MiB/s 8.8855 MiB/s 8.8916 MiB/s]
                 change:
                        time:   [-1.4567% -1.2851% -1.1150%] (p = 0.00 < 0.05)
                        thrpt:  [+1.1275% +1.3019% +1.4782%]
                        Performance has improved.
Found 12 outliers among 100 measurements (12.00%)
  6 (6.00%) high mild
  6 (6.00%) high severe
linter/all-rules/numpy/ctypeslib.py
                        time:   [1.6182 ms 1.6188 ms 1.6196 ms]
                        thrpt:  [10.281 MiB/s 10.286 MiB/s 10.290 MiB/s]
                 change:
                        time:   [-1.8875% -1.6560% -1.4401%] (p = 0.00 < 0.05)
                        thrpt:  [+1.4611% +1.6839% +1.9238%]
                        Performance has improved.
Found 7 outliers among 100 measurements (7.00%)
  3 (3.00%) high mild
  4 (4.00%) high severe
linter/all-rules/large/dataset.py
                        time:   [6.8538 ms 6.8566 ms 6.8596 ms]
                        thrpt:  [5.9308 MiB/s 5.9334 MiB/s 5.9358 MiB/s]
                 change:
                        time:   [-1.7648% -1.4304% -1.2235%] (p = 0.00 < 0.05)
                        thrpt:  [+1.2386% +1.4511% +1.7965%]
                        Performance has improved.
Found 12 outliers among 100 measurements (12.00%)
  7 (7.00%) high mild
  5 (5.00%) high severe
```


---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-05 00:53_

---

_@charliermarsh reviewed on 2023-05-05 00:54_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/branch_detection.rs`:113 on 2023-05-05 00:54_

(This got moved into `ruff_python_semantic`, since it now depends on `Nodes`.)

---

_Comment by @github-actions[bot] on 2023-05-05 01:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.6±0.06ms     2.8 MB/sec    1.01     14.8±0.04ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.01      3.6±0.03ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    360.3±0.86µs     8.2 MB/sec    1.03    372.8±1.88µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.02      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.03ms     5.5 MB/sec    1.04      7.7±0.01ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1534.8±4.02µs    10.8 MB/sec    1.03  1578.4±21.87µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.1±0.93µs    18.0 MB/sec    1.04    171.4±0.24µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.8 MB/sec    1.02      3.3±0.01ms     7.6 MB/sec
parser/large/dataset.py                    1.00      6.0±0.01ms     6.8 MB/sec    1.00      6.0±0.00ms     6.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1161.0±1.34µs    14.3 MB/sec    1.00   1160.4±2.26µs    14.3 MB/sec
parser/numpy/globals.py                    1.01    118.9±0.36µs    24.8 MB/sec    1.00    118.2±0.54µs    25.0 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.1 MB/sec    1.00      2.5±0.01ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.5±0.36ms     2.5 MB/sec    1.00     16.3±0.18ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.08ms     4.0 MB/sec    1.00      4.1±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    489.2±8.90µs     6.0 MB/sec    1.00    487.5±7.93µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.12ms     3.7 MB/sec    1.00      7.0±0.19ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.4±0.12ms     4.9 MB/sec    1.00      8.3±0.10ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1772.7±27.78µs     9.4 MB/sec    1.00  1759.3±24.40µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.8±5.28µs    14.9 MB/sec    1.01    199.0±7.95µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.05ms     6.8 MB/sec    1.00      3.7±0.07ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.7±0.07ms     6.1 MB/sec    1.00      6.7±0.08ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1279.4±22.05µs    13.0 MB/sec    1.00  1277.6±22.12µs    13.0 MB/sec
parser/numpy/globals.py                    1.00    131.8±2.93µs    22.4 MB/sec    1.00    131.5±2.13µs    22.4 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.04ms     9.0 MB/sec    1.00      2.9±0.04ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:3622 on 2023-05-05 06:31_

Does `node_id` match the id of the current stmt? If so, how about `self.ctx.current_stmt_id()`? It would clarify the relationship to `current_stmt` 

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/deferred.rs`:13 on 2023-05-05 06:32_

Nit: Can we add some documentation to the type. To what node does the  `NodeId` point to?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/types.rs`:84 on 2023-05-05 06:36_

Nit: Is it possible to use a wildcard implementation here or do we intentionally restrict the type to `Stmt` and `Expr`?
```suggestion
impl<'a, T> From<RefEquality<'a, T>> for &'a T {
    fn from(r: RefEquality<'a, T>) -> Self {
        r.0
    }
}
```


---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/node.rs`:51 on 2023-05-05 06:43_

I think we should either panic if the `node` was already present in the map OR return the existing id (use the `entry` API)

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/node.rs`:49 on 2023-05-05 06:44_

Nit: `Nodes` acts more like a `Map` than a `Vec` or `Stack`. Should we rename the method to `insert` or `add`?

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/node.rs`:14 on 2023-05-05 06:47_

I wonder if we should use `NonZeroU32` here (and either offset the `indices` when accessing the `nodes` vec or add a dummy node to the vec). The benefit of doing so is that it would shrink the `Option<NodeId>` from 8 bytes to 4 bytes. 

Or we introduce a new, module internal, `ParentId` that uses `NonZeroU32` internally that can be converted from/to a `NodeId`. This requires some more boilerplate but may be easier to implement (but limits the benefits of this optimization to this module only).

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/node.rs`:45 on 2023-05-05 06:50_

I wonder if we should rename `NodeId` to `StmtId` and `Nodes` to `Statements` since the API only accepts `Stmt`s as `node` parameters.

---

_@MichaReiser approved on 2023-05-05 06:51_

🎸🎸🎸

---

_@charliermarsh reviewed on 2023-05-05 19:34_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/node.rs`:45 on 2023-05-05 19:34_

I'm considering making this generic and using the same abstraction for expression-tracking.

---

_@charliermarsh reviewed on 2023-05-06 15:48_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/types.rs`:84 on 2023-05-06 15:48_

Apparently not:

```
error[E0210]: type parameter `T` must be covered by another type when it appears before the first local type (`RefEquality<'a, T>`)
  --> crates/ruff_python_ast/src/types.rs:74:10
   |
74 | impl<'a, T> From<RefEquality<'a, T>> for &'a T {
   |          ^ type parameter `T` must be covered by another type when it appears before the first local type (`RefEquality<'a, T>`)
   |
   = note: implementing a foreign trait is only possible if at least one of the types for which it is implemented is local, and no uncovered type parameters appear before that first local type
   = note: in this case, 'before' refers to the following order: `impl<..> ForeignTrait<T1, ..., Tn> for T0`, where `T0` is the first and `Tn` is the last

For more information about this error, try `rustc --explain E0210`.
error: could not compile `ruff_python_ast` due to previous error
```

---

_@charliermarsh reviewed on 2023-05-06 16:07_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/node.rs`:14 on 2023-05-06 16:07_

Gave it a shot!

---

_Merged by @charliermarsh on 2023-05-06 16:12_

---

_Closed by @charliermarsh on 2023-05-06 16:12_

---

_Branch deleted on 2023-05-06 16:12_

---
