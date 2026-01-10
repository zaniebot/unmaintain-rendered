```yaml
number: 12490
title: "Ignore `NPY201` inside `except` blocks for compatibility with older numpy versions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
assignees: []
merged: true
base: main
head: alex/numpy-depr
created_at: 2024-07-24T12:32:45Z
updated_at: 2024-08-02T16:08:58Z
url: https://github.com/astral-sh/ruff/pull/12490
synced_at: 2026-01-10T21:47:02Z
```

# Ignore `NPY201` inside `except` blocks for compatibility with older numpy versions

---

_Pull request opened by @AlexWaygood on 2024-07-24 12:32_

## Summary

Fixes #12476.

This PR makes it so that deprecated imports and attribute accesses of numpy members are ignored in the following conditions:

- For attribute accesses (e.g. `np.ComplexWarning`), we only ignore the violation if it's inside an `except AttributeError` block, and the member is accessed through its non-deprecated name in the associated `try` block.
- For uses of the `numpy` member where it's simply an `ExprName` node, we check to see how the `numpy` member was bound. If it was bound via a `from numpy import foo` statement, we check to see if that import statement took place inside an `except ImportError` or `except ModuleNotFoundError` block. If so, and if the `numpy` member was imported through its non-deprecated name in the associated try block, we ignore the violation in the same way.

Examples:

```py
import numpy as np

try:
    np.all([True, True])
except AttributError:
    np.alltrue([True, True])  # Okay

try:
    from numpy.exceptions import ComplexWarning
except ImportError:
    from numpy import ComplexWarning

x = ComplexWarning()  # Okay
```

This was more complex than I expected, since we currently track _handled_ exceptions in the semantic model, but we don't track _suspended_ exceptions. I.e., if we're in a lint rule and we're examining a node inside a `try` block, we know which exceptions the enclosing `StmtTry` node handles. But if we're in a lint rule and we're examining a node inside an `except` block, we don't have that information easily to hand.

## Test Plan

Added some fixtures and snapshots.


---

_Label `rule` added by @AlexWaygood on 2024-07-24 12:32_

---

_Comment by @codspeed-hq[bot] on 2024-07-24 12:47_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex/numpy-depr)

### Merging #12490 will **not alter performance**

<sub>Comparing <code>alex/numpy-depr</code> (9c8c7d3) with <code>alex/numpy-depr</code> (bb14c21)</sub>



### Summary

`✅ 33` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-07-24 12:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-07-24 13:32_

The lexer benchmark is drunk! 

---

_Comment by @AlexWaygood on 2024-07-24 13:34_

> The lexer benchmark is drunk!

Hey, I'll take it!

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/numpy/rules/numpy_2_0_deprecation.rs`:761 on 2024-07-24 13:36_

Can we check `semantic.handled_exceptions` instead? Does that work? Then we wouldn't need to find the try statement or anything.

---

_@charliermarsh reviewed on 2024-07-24 13:36_

---

_@AlexWaygood reviewed on 2024-07-24 13:37_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/numpy/rules/numpy_2_0_deprecation.rs`:761 on 2024-07-24 13:37_

No, `semantic.handled_exceptions` records the exceptions if we're visiting the `try` block of a `try`/`except` statement. But here we're visiting the `except` block of a `try`/`except` statement. Hence "suspended_exceptions" rather than "handled_exceptions".

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/numpy/rules/numpy_2_0_deprecation.rs`:902 on 2024-07-24 13:37_

I don't think it matters much because we only run this code in the error case, but it is kind of expensive. Do we have no way of resolving the statement in which a binding is declared and then traverse upwards (by using the parent API?)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/numpy/rules/numpy_2_0_deprecation.rs`:940 on 2024-07-24 13:38_

Nit: Add an empty `visit_expression` to avoid traversing **into** expressions or implement `StatementVisitor`

---

_@AlexWaygood reviewed on 2024-07-24 13:39_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/numpy/rules/numpy_2_0_deprecation.rs`:761 on 2024-07-24 13:39_

Also we still need to find the node for the `try` statement so that we can check whether it was accessed via the undeprecated call path in the `try` block

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/numpy/rules/numpy_2_0_deprecation.rs`:847 on 2024-07-24 13:42_

Nit: It could make sense to add a `visit_stmt` implementation that short-circuits visiting when `self.found_attribute` is true (or `visit_body`)

---

_@MichaReiser approved on 2024-07-24 13:42_

Looks good. Maybe give @charliermarsh a grace period of a day to look at the semantic model changes ;)

---

_@AlexWaygood reviewed on 2024-07-24 13:47_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/numpy/rules/numpy_2_0_deprecation.rs`:902 on 2024-07-24 13:47_

The situation is that we're visiting an `except` block in a `try`/`except` statement. We find a deprecated numpy access in that `except` block. We need to find out if there's an undeprecated access in the `try` block of the `try`/`except` statement. The way I do this is by traversing upwards from the node we're linting in the `except` block (using the parent API) to find the `StmtTry` node, then from the `StmtTry` node I then traverse downwards into the `try` block using the `ImportSearcher` to find if there are any imports using the undeprecated API.

---

_@AlexWaygood reviewed on 2024-07-24 14:07_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/numpy/rules/numpy_2_0_deprecation.rs`:847 on 2024-07-24 14:07_

Thanks, I pushed that change! Wasn't quite sure what best practices were here, since there's a lot of `visit_*` methods on the `Visitor` trait that could potentially be overridden here to short-circuit. I guess most of the other ones generally delegate to `visit_expr` and `visit_stmt`?

---

_@MichaReiser reviewed on 2024-07-24 15:08_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/numpy/rules/numpy_2_0_deprecation.rs`:847 on 2024-07-24 15:08_

Yeah. I think `visit_stmt` makes sense because it short circuits early. 

---

_@charliermarsh reviewed on 2024-07-24 19:41_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/mod.rs`:1846 on 2024-07-24 19:41_

Idly wondering if we should just store the semantic model flags on the binding, in theory it's tiny.

---

_@charliermarsh reviewed on 2024-07-24 19:42_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/numpy/rules/numpy_2_0_deprecation.rs`:806 on 2024-07-24 19:42_

Could you instead add a method `statements` to the semantic model that's identical to the `pub fn expressions(&self, node_id: NodeId) -> impl Iterator<Item = &'a Expr> + '_` variant? Or is this doing something else?

---

_@AlexWaygood reviewed on 2024-07-24 19:43_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/mod.rs`:1846 on 2024-07-24 19:43_

As in, get rid of the separate `BindingFlags` struct altogether, and store the semantic-model flags on `Binding` instead? Or store the semantic-model flags in addition to the `BindingFlags`?

---

_@charliermarsh reviewed on 2024-07-24 19:43_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/numpy/rules/numpy_2_0_deprecation.rs`:835 on 2024-07-24 19:43_

I think you can just call `visit_body`, or is this an optimization?

---

_@charliermarsh approved on 2024-07-24 19:43_

---

_@charliermarsh reviewed on 2024-07-24 19:45_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/numpy/rules/numpy_2_0_deprecation.rs`:835 on 2024-07-24 19:45_

Oh sorry I see there was some discussion here that got resolved, feel free to ignore.

---

_@charliermarsh reviewed on 2024-07-24 19:45_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/mod.rs`:1846 on 2024-07-24 19:45_

Store them in addition (and then remove this flag from `BindingFlags`). But, it's ok, no need to change.

---

_@AlexWaygood reviewed on 2024-07-24 19:46_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/numpy/rules/numpy_2_0_deprecation.rs`:835 on 2024-07-24 19:46_

naw this is a good point

---

_@AlexWaygood reviewed on 2024-07-24 19:58_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/numpy/rules/numpy_2_0_deprecation.rs`:806 on 2024-07-24 19:58_

Ahh, nice idea

---

_Merged by @AlexWaygood on 2024-07-24 20:03_

---

_Closed by @AlexWaygood on 2024-07-24 20:03_

---

_Branch deleted on 2024-07-24 20:03_

---
