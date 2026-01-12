```yaml
number: 5324
title: Fix annotation and format spec visitors
type: pull_request
state: merged
author: jgberry
labels: []
assignees: []
merged: true
base: main
head: fix-visit-annotation
created_at: 2023-06-23T03:34:58Z
updated_at: 2023-06-23T04:45:37Z
url: https://github.com/astral-sh/ruff/pull/5324
synced_at: 2026-01-12T15:55:18Z
```

# Fix annotation and format spec visitors

---

_@jgberry_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The `Visitor` and `preorder::Visitor` traits provide some convenience functions, `visit_annotation` and `visit_format_spec`, for handling annotation and format spec expressions respectively. Both of these functions accept an `&Expr` and have a default implementation which delegates to `walk_expr`. The problem with this approach is that any custom handling done in `visit_expr` will be skipped for annotations and format specs. Instead, to capture any custom logic implemented in `visit_expr`, both of these function's default implementations should delegate to `visit_expr` instead of `walk_expr`.

## Example

Consider the below `Visitor` implementation:
```rust
impl<'a> Visitor<'a> for Example<'a> {
    fn visit_expr(&mut self, expr: &'a Expr) {
        match expr {
            Expr::Name(ExprName { id, .. }) => println!("Visiting {:?}", id),
            _ => walk_expr(self, expr),
        }
    }
}
```

Run on the following Python snippet:
```python
a: b
```

I would expect such a visitor to print the following:
```
Visiting b
Visiting a
```

But it instead prints the following:
```
Visiting a
```

Our custom `visit_expr` handler is not invoked for the annotation.

## Test Plan

Tests added in #5271 caught this behavior.


---

_@charliermarsh reviewed on 2023-06-23 03:36_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/visitor.rs`:56 on 2023-06-23 03:36_

I know it's sort of dumb / tedious, but wondering if we should instead add `walk_format_spec` that just calls `visitor.visit_expr(format_spec)`, for consistency with the rest of the trait.

---

_@jgberry reviewed on 2023-06-23 03:44_

---

_Review comment by @jgberry on `crates/ruff_python_ast/src/visitor.rs`:56 on 2023-06-23 03:44_

I can see the argument for that. Added in 125f33c34c340ac8d42196cfa674701711050551.

---

_Review requested from @charliermarsh by @jgberry on 2023-06-23 03:47_

---

_@charliermarsh reviewed on 2023-06-23 03:51_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/visitor.rs`:56 on 2023-06-23 03:51_

Thanks. I think it will be clearer coming back to this code in the future, rather than having to ask myself, "Why does this use `self` unlike all the others?"

---

_@charliermarsh approved on 2023-06-23 03:51_

---

_Merged by @charliermarsh on 2023-06-23 03:55_

---

_Closed by @charliermarsh on 2023-06-23 03:55_

---

_Comment by @github-actions[bot] on 2023-06-23 03:59_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.9±0.01ms     5.9 MB/sec    1.00      6.9±0.01ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1461.5±3.97µs    11.4 MB/sec    1.01   1470.3±1.26µs    11.3 MB/sec
formatter/numpy/globals.py                 1.00    161.0±1.59µs    18.3 MB/sec    1.00    161.3±0.17µs    18.3 MB/sec
formatter/pydantic/types.py                1.01      3.5±0.01ms     7.3 MB/sec    1.00      3.4±0.01ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.5±0.02ms     3.0 MB/sec    1.01     13.6±0.03ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.8 MB/sec    1.00      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.02    368.2±9.48µs     8.0 MB/sec    1.00    362.4±0.71µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.01ms     4.2 MB/sec    1.01      6.0±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.01ms     5.8 MB/sec    1.01      7.0±0.01ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1461.6±2.45µs    11.4 MB/sec    1.01   1477.1±2.78µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    157.4±0.15µs    18.7 MB/sec    1.00    157.8±0.16µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.00      3.2±0.00ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.20     14.2±0.62ms     2.9 MB/sec    1.00     11.9±0.58ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.17      2.9±0.19ms     5.8 MB/sec    1.00      2.5±0.19ms     6.8 MB/sec
formatter/numpy/globals.py                 1.13   304.6±20.66µs     9.7 MB/sec    1.00   270.5±24.93µs    10.9 MB/sec
formatter/pydantic/types.py                1.16      6.4±0.32ms     4.0 MB/sec    1.00      5.5±0.34ms     4.6 MB/sec
linter/all-rules/large/dataset.py          1.01     22.3±0.82ms  1865.8 KB/sec    1.00     22.1±0.83ms  1883.0 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      5.9±0.34ms     2.8 MB/sec    1.00      5.7±0.31ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   701.1±62.72µs     4.2 MB/sec    1.00   698.2±44.92µs     4.2 MB/sec
linter/all-rules/pydantic/types.py         1.03     10.1±0.49ms     2.5 MB/sec    1.00      9.8±0.51ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.02     11.7±0.59ms     3.5 MB/sec    1.00     11.4±0.54ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.5±0.13ms     6.7 MB/sec    1.00      2.4±0.13ms     6.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   289.0±19.11µs    10.2 MB/sec    1.02   296.1±35.19µs    10.0 MB/sec
linter/default-rules/pydantic/types.py     1.02      5.1±0.35ms     5.0 MB/sec    1.00      5.0±0.24ms     5.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Branch deleted on 2023-06-23 04:45_

---
