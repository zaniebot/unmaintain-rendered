```yaml
number: 5927
title: Implement visitation of type aliases and parameters
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/695-visitor
created_at: 2023-07-20T17:17:05Z
updated_at: 2023-07-25T17:32:30Z
url: https://github.com/astral-sh/ruff/pull/5927
synced_at: 2026-01-12T03:30:22Z
```

# Implement visitation of type aliases and parameters

---

_Pull request opened by @zanieb on 2023-07-20 17:17_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of #5062 
Requires https://github.com/astral-sh/RustPython-Parser/pull/32

Adds visitation of type alias statements and type parameters in class and function definitions. 

Duplicates tests for `PreorderVisitor` into `Visitor` with new snapshots. Testing required node implementations for the `TypeParam` enum, which is a chunk of the diff and the reason we need `Ranged` implementations in https://github.com/astral-sh/RustPython-Parser/pull/32.

## Test Plan

<!-- How was it tested? -->

Adds unit tests with snapshots.


---

_Comment by @github-actions[bot] on 2023-07-20 17:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.3±0.61ms     4.0 MB/sec    1.06     10.9±0.15ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.2±0.03ms     7.6 MB/sec    1.00      2.1±0.05ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    236.4±6.55µs    12.5 MB/sec    1.08   254.7±16.18µs    11.6 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.15ms     5.6 MB/sec    1.00      4.5±0.13ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.00     14.6±0.43ms     2.8 MB/sec    1.01     14.8±0.21ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.07ms     4.4 MB/sec    1.00      3.8±0.04ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    493.3±6.55µs     6.0 MB/sec    1.00    492.4±9.35µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.04      6.8±0.10ms     3.8 MB/sec    1.00      6.5±0.24ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.6±0.12ms     5.3 MB/sec    1.00      7.6±0.18ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1652.6±17.93µs    10.1 MB/sec    1.00  1609.1±42.32µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.8±7.02µs    17.0 MB/sec    1.01    175.3±6.08µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.13ms     7.6 MB/sec    1.02      3.4±0.12ms     7.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.8±0.05ms     3.8 MB/sec    1.00     10.8±0.08ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.1±0.02ms     7.8 MB/sec    1.00      2.1±0.02ms     7.9 MB/sec
formatter/numpy/globals.py                 1.00    229.7±2.97µs    12.8 MB/sec    1.00    229.2±3.09µs    12.9 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.03ms     5.4 MB/sec    1.00      4.7±0.04ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.2±0.13ms     2.7 MB/sec    1.01     15.4±0.11ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.03ms     4.1 MB/sec    1.01      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    409.1±6.98µs     7.2 MB/sec    1.01    413.8±6.73µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.06ms     3.7 MB/sec    1.03      7.1±0.09ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.05ms     5.1 MB/sec    1.01      8.1±0.05ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1616.8±12.96µs    10.3 MB/sec    1.02  1641.9±14.93µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.7±1.62µs    17.0 MB/sec    1.00    174.5±2.06µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.08ms     7.0 MB/sec    1.01      3.7±0.04ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Converted to draft by @zanieb on 2023-07-20 17:57_

---

_@zanieb reviewed on 2023-07-20 18:04_

---

_Review comment by @zanieb on `Cargo.toml`:59 on 2023-07-20 18:04_

This pin is temporary and will need to be amended on merge of https://github.com/astral-sh/RustPython-Parser/pull/32

---

_@zanieb reviewed on 2023-07-20 18:05_

---

_Review comment by @zanieb on `crates/ruff_python_ast/src/snapshots/ruff_python_ast__visitor__tests__class_type_parameters.snap`:2 on 2023-07-20 18:05_

relevant test

---

_@zanieb reviewed on 2023-07-20 18:06_

---

_Review comment by @zanieb on `crates/ruff_python_ast/src/visitor/preorder.rs`:851 on 2023-07-20 18:06_

These just contain an identifier and range so there is nothing to visit?

---

_@zanieb reviewed on 2023-07-20 18:09_

---

_Review comment by @zanieb on `crates/ruff_python_ast/src/snapshots/ruff_python_ast__visitor__tests__function_type_parameters.snap`:2 on 2023-07-20 18:09_

relevant test

---

_@zanieb reviewed on 2023-07-20 18:09_

---

_Review comment by @zanieb on `crates/ruff_python_ast/src/snapshots/ruff_python_ast__visitor__tests__compare.snap`:2 on 2023-07-20 18:09_

Most of these snapshots are new coverage of previous behavior with tests duplicated from the pre-order visitor. 

---

_@zanieb reviewed on 2023-07-20 18:09_

---

_Review comment by @zanieb on `crates/ruff_python_ast/src/snapshots/ruff_python_ast__visitor__tests__type_aliases.snap`:2 on 2023-07-20 18:09_

relevant test

---

_Review requested from @charliermarsh by @zanieb on 2023-07-20 18:17_

---

_@charliermarsh reviewed on 2023-07-21 00:03_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/visitor/preorder.rs`:851 on 2023-07-21 00:03_

Sounds right to me.

---

_@charliermarsh reviewed on 2023-07-21 00:07_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/visitor.rs`:970 on 2023-07-21 00:07_

Is this identical to the version in `preorder.rs`? Is there any way we can share these utilities?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/visitor.rs`:712 on 2023-07-21 06:40_

Nit
```suggestion
        TypeParam::TypeVarTuple(_) | TypeParam::ParamSpec(_) => {}
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/visitor.rs`:970 on 2023-07-21 06:43_

It implements a different `Visitor` trait. But we could implement both `Visitor` traits for `RecordVisitor` and share the `enter_node`, `exit_node`, and `emit` functionality. 

One thing that we could consider adding (out of scope for this PR) is that we have a `pre_visit_node(node: AnyNodeRef)` and maybe `post_visit_node(node: AnyNodeRef)` visitor functions. This would make it easier to implement `Visitor`'s like this (or the comments extraction visitor in the formatter) that want to visit all nodes.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/visitor.rs`:713 on 2023-07-21 06:45_

I don't know if any visitor overrides the function but we should call `visit_expr_context`:

```rust
Expr::Name(ast::ExprName { ctx, .. }) => {
	visitor.visit_expr_context(ctx);
}
```

---

_@MichaReiser approved on 2023-07-21 06:48_

LGTM.

Can you search for all `Visitor` (preorder or normal) implementations and add the overrides for `visit_type_params` for visitors that must visit all nodes or do you plan to do this as separate PRS? 

For example, it's important for the correctness of `CommentsVisitor` to override every node visiting function:

https://github.com/astral-sh/ruff/blob/59f59403a07ba0a5398bba830a4eef118bf84aea/crates/ruff_python_formatter/src/comments/visitor.rs#L154

---

_@zanieb reviewed on 2023-07-21 14:43_

---

_Review comment by @zanieb on `crates/ruff_python_ast/src/visitor.rs`:970 on 2023-07-21 14:43_

The biggest problem here was `visit/walk_module` was missing from `Visitor` but present in `PreorderVisitor`. We can explore a consolidated test pattern in a follow-up. I'll make sure there's a tracking issue before this is merged.

---

_@zanieb reviewed on 2023-07-24 17:53_

---

_Review comment by @zanieb on `crates/ruff_python_ast/src/visitor.rs`:713 on 2023-07-24 17:53_

`name` here is an `Identifier` not a `Expr::Name` with context. Did you mean somewhere else?

---

_Comment by @zanieb on 2023-07-24 17:56_

> Can you search for all Visitor (preorder or normal) implementations and add the overrides for visit_type_params for visitors that must visit all nodes or do you plan to do this as separate PRS?


I'll do this here!


---

_Marked ready for review by @zanieb on 2023-07-24 20:29_

---

_@MichaReiser reviewed on 2023-07-25 06:52_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/visitor.rs`:713 on 2023-07-25 06:52_

Oh, my mistake. The fact that `name` is an identifier might be a bit misleading (this is unrelated to this PR).

---

_Merged by @zanieb on 2023-07-25 17:11_

---

_Closed by @zanieb on 2023-07-25 17:11_

---

_Branch deleted on 2023-07-25 17:11_

---
