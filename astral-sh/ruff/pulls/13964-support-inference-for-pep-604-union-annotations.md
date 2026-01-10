```yaml
number: 13964
title: Support inference for PEP 604 union annotations
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/union
created_at: 2024-10-28T13:11:06Z
updated_at: 2024-10-28T14:21:22Z
url: https://github.com/astral-sh/ruff/pull/13964
synced_at: 2026-01-10T20:59:37Z
```

# Support inference for PEP 604 union annotations

---

_Pull request opened by @charliermarsh on 2024-10-28 13:11_

## Summary

Supports return type inference for, e.g., `def f() -> int | None:`.


---

_Review requested from @carljm by @charliermarsh on 2024-10-28 13:11_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-10-28 13:11_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-10-28 13:11_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/assignment/annotations.md`:26 on 2024-10-28 13:12_

```suggestion
## PEP-604 annotations are supported
```

---

_@AlexWaygood reviewed on 2024-10-28 13:12_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:3499 on 2024-10-28 13:12_

I struggled with this a bit -- should this line still be here? If it's left as-is, we hit the `assert!(previous.is_none());` assertion below. If we leave it as-is but use `let left_ty = self.expression_ty(&binary.left);` instead of `let left_ty = self.infer_type_expression(&binary.left);`, then we get inferences like `Literal[int] | Literal[None]`.

---

_@charliermarsh reviewed on 2024-10-28 13:12_

---

_Label `red-knot` added by @charliermarsh on 2024-10-28 13:12_

---

_@AlexWaygood reviewed on 2024-10-28 13:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3499 on 2024-10-28 13:17_

I think the way you've done it in this PR is correct!

The invariant is that we should only insert a type into `self.types.expressions` _once_ for each AST node in the tree. What `self.infer_binary_expression()` does here is it iterates through all the subnodes and stores a type in `self.types.expressions` for each subnode. But that's incorrect with the changes you're making here, because we're calling `self.infer_type_expression` on each subnode, and `self.infer_type_expression` already stores a type in `self.types.expressions` for each subnode (it's the `let previous = self.types.expressions.insert(expr_id, ty);` line at the bottom of this method).

---

_@AlexWaygood reviewed on 2024-10-28 13:19_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3498 on 2024-10-28 13:19_

using `if`/`let` might be okay here. I don't think there are any other binary operations that are legal in type annotations right now, so it's unlikely we'll ever add any other branches in the medium term. In the long term, intersection types might eventually get standardised, which would mean `str & int` would become a legal annotation... but I think that's still a long way off for now.

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:3498 on 2024-10-28 13:20_

I personally still find this clearer given the pattern-matching style of the file.

---

_@charliermarsh reviewed on 2024-10-28 13:20_

---

_@AlexWaygood reviewed on 2024-10-28 13:22_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3498 on 2024-10-28 13:22_

Fair. I'm not a massive fan of this Clippy rule in general TBH -- it feels like it has too many false positives for my liking

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/assignment/annotations.md`:30 on 2024-10-28 13:28_

Could you also add a test that uses a union with duplicate elements, e.g. `str | int | str | None`? We should infer the type as `str | int | None` (we should deduplicate the elements). I think your PR does that, but it would be great to verify it with an explicit test.

---

_Comment by @github-actions[bot] on 2024-10-28 13:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-10-28 13:32_

LGTM! You'll need to update the list of expected diagnostics in our red-knot benchmark here -- we have an assertion in the benchmark that checks that exactly these diagnostics are emitted when benchmarking red-knot with `tomllib`.

https://github.com/astral-sh/ruff/blob/c593ccb529a8183d5016467adecf25b0981a31ad/crates/ruff_benchmark/benches/red_knot.rs#L25-L49

I *think* these are false positives caused by a pre-existing issue (exposed by your PR) that we need to address. But I don't think that should block your PR from being merged, since the bug was already present (just hidden)

---

_Merged by @charliermarsh on 2024-10-28 14:13_

---

_Closed by @charliermarsh on 2024-10-28 14:13_

---

_Branch deleted on 2024-10-28 14:13_

---
