```yaml
number: 6345
title: Store expression hierarchy in semantic model snapshots
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/expressions
created_at: 2023-08-04T16:23:57Z
updated_at: 2023-08-15T01:39:27Z
url: https://github.com/astral-sh/ruff/pull/6345
synced_at: 2026-01-12T15:55:21Z
```

# Store expression hierarchy in semantic model snapshots

---

_@charliermarsh_

## Summary

When we iterate over the AST for analysis, we often process nodes in a "deferred" manner. For example, if we're analyzing a function, we push the function body onto a deferred stack, along with a snapshot of the current semantic model state. Later, when we analyze the body, we restore the semantic model state from the snapshot. This ensures that we know the correct scope, hierarchy of statement parents, etc., when we go to analyze the function body.

Historically, we _haven't_ included the _expression_ hierarchy in the model snapshot -- so we track the current expression parents in the visitor, but we never save and restore them when processing deferred nodes. This can lead to subtle bugs, in that methods like `expr_parent()` aren't guaranteed to be correct, if you're in a deferred visitor.

This PR migrates expression tracking to mirror statement tracking exactly. So we push all expressions onto an `IndexVec`, and include the current expression on the snapshot. This ensures that `expr_parent()` and related methods are "always correct" rather than "sometimes correct".

There's a performance cost here, both at runtime and in terms of memory consumption (we now store an additional pointer for every expression). In my hyperfine testing, it's about a 1% performance decrease for all-rules on CPython (up to 533.8ms, from 528.3ms) and a 4% performance decrease for default-rules on CPython (up to 212ms, from 204ms). However... I think this is worth it given the incorrectness of our current approach. In the future, we may want to reconsider how we do these upward traversals (e.g., with something like a red-green tree). (**Note**: in https://github.com/astral-sh/ruff/pull/6351, the slowdown seems to be entirely removed.)


---

_Comment by @charliermarsh on 2023-08-04 16:24_

This will also fix https://github.com/astral-sh/ruff/issues/6285, but I'll handle that in a separate PR, I want to update the fixtures and it would be too noisy here.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-04 16:29_

---

_Comment by @github-actions[bot] on 2023-08-04 16:50_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.2±0.15ms     4.9 MB/sec    1.00      8.2±0.13ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1632.6±39.05µs    10.2 MB/sec    1.00  1634.7±39.80µs    10.2 MB/sec
formatter/numpy/globals.py                 1.00    182.8±6.00µs    16.1 MB/sec    1.01    184.1±7.57µs    16.0 MB/sec
formatter/pydantic/types.py                1.01      3.5±0.09ms     7.4 MB/sec    1.00      3.4±0.05ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.00     11.0±0.07ms     3.7 MB/sec    1.02     11.3±0.16ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.01ms     6.0 MB/sec    1.00      2.8±0.01ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    384.1±0.57µs     7.7 MB/sec    1.01    387.1±0.66µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.0±0.05ms     5.1 MB/sec    1.02      5.1±0.03ms     5.0 MB/sec
linter/default-rules/large/dataset.py      1.00      5.4±0.03ms     7.6 MB/sec    1.02      5.5±0.03ms     7.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1139.3±5.59µs    14.6 MB/sec    1.02   1157.0±6.87µs    14.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    127.5±1.16µs    23.1 MB/sec    1.03    131.1±3.65µs    22.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.01ms    10.7 MB/sec    1.02      2.4±0.02ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     13.2±0.70ms     3.1 MB/sec    1.00     13.3±0.73ms     3.1 MB/sec
formatter/numpy/ctypeslib.py               1.06      2.7±0.29ms     6.2 MB/sec    1.00      2.6±0.16ms     6.5 MB/sec
formatter/numpy/globals.py                 1.00   293.5±33.83µs    10.1 MB/sec    1.01   297.9±56.58µs     9.9 MB/sec
formatter/pydantic/types.py                1.00      5.5±0.32ms     4.7 MB/sec    1.01      5.5±0.27ms     4.6 MB/sec
linter/all-rules/large/dataset.py          1.00     18.3±0.78ms     2.2 MB/sec    1.04     19.1±0.86ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.27ms     3.5 MB/sec    1.02      4.9±0.26ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   582.3±30.81µs     5.1 MB/sec    1.00   579.5±40.79µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.03      8.6±0.47ms     3.0 MB/sec    1.00      8.3±0.52ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.02     10.0±0.63ms     4.1 MB/sec    1.00      9.8±0.45ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1922.4±98.52µs     8.7 MB/sec    1.01  1932.4±77.76µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   216.2±19.84µs    13.6 MB/sec    1.06   228.8±25.06µs    12.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.2±0.26ms     6.0 MB/sec    1.06      4.5±0.39ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-08-04 18:07_

Hmm, we don't need back references for expressions (we never need to be able to go from a node to its ID, or from a node to its parent -- we can always go from an ID to its parent), so perhaps we can improve performance here by omitting the hash map from `Nodes` in that case. I will test it out.

---

_Comment by @charliermarsh on 2023-08-04 18:36_

Ok, I'm going to resolve the performance degradation in a separate PR because I think it's easier to review this as-is.

---

_Label `internal` added by @charliermarsh on 2023-08-04 18:44_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_pytest_style/rules/assertion.rs`:251 on 2023-08-07 07:14_

Is this change intentional?

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:43 on 2023-08-07 07:17_

Nit: We now store both a stack of statements and expressions. This gives us almost a full fidelity parent chain, except that it isn't possible to get the `parent` statement of an expression (except for the current expression). 

What do you think of introducing a `Vec<AnyNodeRef>` that stores all parent nodes (Ultimately means `O(n)` pointers to be written, where `n` = len(AST)) instead? You can retrieve the current statement by traversing upwards until you find the first `is_statement()` node. 

The one issue of that is that there's currently no way to go from `AnyNodeRef` to `&Stmt`. We would need to introduce a `AnyStatementRef` enum for that.

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:864 on 2023-08-07 07:19_

Why is it necessary to `skip(1)`? Shouldn't `ancestor_ids` return the ancestors only?

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:867 on 2023-08-07 07:20_

Nit: Use successors? 

```suggestion
std::iter::successors(self.expr_id, |id| self.exprs.ancestor_ids(id).skip(1).map(|id| self.exprs[id])))
```

---

_@MichaReiser approved on 2023-08-07 07:20_

---

_Comment by @MichaReiser on 2023-08-07 07:21_

Could we add some tests for the new introduced APIs? Or how did you test that this works as expected?

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:43 on 2023-08-07 13:34_

I will consider this separately, it makes sense to me, although it may come with the performance regression that I documented here and fixed in https://github.com/astral-sh/ruff/pull/6351.

---

_@charliermarsh reviewed on 2023-08-07 13:34_

---

_@charliermarsh reviewed on 2023-08-07 13:35_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pytest_style/rules/assertion.rs`:251 on 2023-08-07 13:35_

Yes! Not necessary. See the identical change in `crates/ruff/src/rules/pandas_vet/rules/inplace_argument.rs` that removes the large comment around Case 3. The fact that these tests still pass is a evidence that this change is working as expected.

---

_@charliermarsh reviewed on 2023-08-07 13:36_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:864 on 2023-08-07 13:36_

Right now, `ancestor_ids` starts with the current ID IIRC, probably because it's convenient with the `successors` API.  Do you find that confusing?

---

_Comment by @charliermarsh on 2023-08-07 13:37_

> Could we add some tests for the new introduced APIs? Or how did you test that this works as expected?

I'm going to add some tests in a separate PR, I wanted to make sure that this had a clean snapshot suite.

---

_@charliermarsh reviewed on 2023-08-07 13:41_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:867 on 2023-08-07 13:41_

This doesn't quite do the same thing. The awkwardness is that `self.expr_id` can be `None`.

---

_Merged by @charliermarsh on 2023-08-07 13:42_

---

_Closed by @charliermarsh on 2023-08-07 13:42_

---

_Branch deleted on 2023-08-07 13:42_

---

_@charliermarsh reviewed on 2023-08-07 13:42_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:864 on 2023-08-07 13:42_

(This is current behavior on `main` so I'll look into it separately.)

---

_@MichaReiser reviewed on 2023-08-07 13:51_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:867 on 2023-08-07 13:51_

I'm not sure if I understand. The first argument to `std::iter::successors` is an `Option`. Both `successors and `expr_id.flat_map` should short circuit if expr_id is `None`.

---

_@charliermarsh reviewed on 2023-08-07 13:57_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:867 on 2023-08-07 13:57_

Oh, I think you just had misplaced parentheses in your suggestion and I misunderstood.

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:43 on 2023-08-15 00:18_

I was looking into this and I want to understand the last paragraph here. Is the suggestion that methods like `SemanticModel#current_statement` would now return `AnyStatementRef`? And so we'd change all consumers of that method to work with `AnyStatementRef` rather than `&Stmt`?

---

_@charliermarsh reviewed on 2023-08-15 00:18_

---

_@charliermarsh reviewed on 2023-08-15 00:18_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:43 on 2023-08-15 00:18_

If so, we also need `AnyExpressionRef`, and I'd ultimately like to store the full-fidelity chain here including `Parameters`, `Arguments`, etc...

---

_@charliermarsh reviewed on 2023-08-15 00:37_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:43 on 2023-08-15 00:37_

E.g., these kinds of changes: https://github.com/astral-sh/ruff/compare/charlie/any-node-ref?expand=1#diff-c2c8422959405016e1fb54ca23edee9716bd06ad64e610ceea9612a8291122fcR1815. It's feasible but a lot of call sites to work through, so I want to make sure I understand what's being suggested before I think on it further.

---

_@charliermarsh reviewed on 2023-08-15 01:39_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:43 on 2023-08-15 01:39_

I might propose that we do this incrementally by adding an internal type to the semantic crate that's like `AnyNodeRef`, but less granular:

```rust
#[derive(Debug, Clone)]
pub enum AnyNodeRef<'a> {
    Stmt(&'a Stmt),
    Expr(&'a Expr),
}

impl<'a> AnyNodeRef<'a> {
    pub fn as_statement(&self) -> Option<&'a Stmt> {
        match self {
            AnyNodeRef::Stmt(stmt) => Some(stmt),
            AnyNodeRef::Expr(_) => None,
        }
    }
}
```

That way, we wouldn't need to change the `SemanticModel` interface at all for now.

---
