```yaml
number: 21050
title: "[ty] Consider domain of BDD when checking whether always satisfiable"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/simplify
created_at: 2025-10-23T18:30:23Z
updated_at: 2025-10-24T17:37:58Z
url: https://github.com/astral-sh/ruff/pull/21050
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Consider domain of BDD when checking whether always satisfiable

---

_Pull request opened by @dcreager on 2025-10-23 18:30_

That PR title might be a bit inscrutable.

Consider the two constraints `T ≤ bool` and `T ≤ int`. Since `bool ≤ int`, by transitivity `T ≤ bool` implies `T ≤ int`. (Every type that is a subtype of `bool` is necessarily also a subtype of `int`.) That means that `T ≤ bool ∧ T ≰ int` is an impossible combination of constraints, and is therefore not a valid input to any BDD. We say that that assignment is not in the _domain_ of the BDD.

The implication `T ≤ bool → T ≤ int` can be rewritten as `T ≰ bool ∨ T ≤ int`. (That's the definition of implication.) If we construct that constraint set in an mdtest, we should get a constraint set that is always satisfiable. Previously, that constraint set would correctly _display_ as `always`, but a `static_assert` on it would fail.

The underlying cause is that our `is_always_satisfied` method would only test if the BDD was the `AlwaysTrue` terminal node. `T ≰ bool ∨ T ≤ int` does not simplify that far, because we purposefully keep around those constraints in the BDD structure so that it's easier to compare against other BDDs that reference those constraints.

To fix this, we need a more nuanced definition of "always satisfied". Instead of evaluating to `true` for _every_ input, we only need it to evaluate to `true` for every _valid_ input — that is, every input in its domain.

---

_Review requested from @carljm by @dcreager on 2025-10-23 18:30_

---

_Review requested from @AlexWaygood by @dcreager on 2025-10-23 18:30_

---

_Review requested from @sharkdp by @dcreager on 2025-10-23 18:30_

---

_Review requested from @MichaReiser by @dcreager on 2025-10-23 18:30_

---

_Label `internal` added by @dcreager on 2025-10-23 18:30_

---

_Label `ty` added by @dcreager on 2025-10-23 18:30_

---

_Comment by @github-actions[bot] on 2025-10-23 18:32_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-23 18:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/constraints.md`:656 on 2025-10-23 18:54_

Ugh... I wanted to display this without simplifying the DNF so that we could see (and test!) that its internal structure is not just the `AlwaysTrue` terminal. But the display simplification rules are important to get consistent output even when BDD variable ordering changes. So this is a flaky assertion, as can be seen in the CI failures.

For now, I think I'm going to back out the parts that let us display BDD domains in the mdtests. The equality test below still lets us test that the domain is not just `AlwaysTrue`, even if we can't display it nicely.

---

_@dcreager reviewed on 2025-10-23 18:54_

---

_@ibraheemdev approved on 2025-10-24 15:32_

Nice! We do something similar in uv when simplifying marker trees based on the `requires-python` range of a project.

---

_Merged by @dcreager on 2025-10-24 17:37_

---

_Closed by @dcreager on 2025-10-24 17:37_

---

_Branch deleted on 2025-10-24 17:37_

---
