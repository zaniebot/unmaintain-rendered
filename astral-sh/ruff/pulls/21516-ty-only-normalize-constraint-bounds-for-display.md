```yaml
number: 21516
title: "[ty] Only normalize constraint bounds for display"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/abnormal
created_at: 2025-11-18T19:38:05Z
updated_at: 2025-11-19T16:49:49Z
url: https://github.com/astral-sh/ruff/pull/21516
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Only normalize constraint bounds for display

---

_Pull request opened by @dcreager on 2025-11-18 19:38_

We were previously normalizing the upper and lower bounds of each constraint when constructing constraint sets. Like in #21463, this was for conflated reasons: It made constraint set displays nicer, since we wouldn't render multiple constraints with obviously equivalent bounds. (Think `T ≤ A & B` and `T ≤ B & A`) But it was also useful for correctness, since prior to #21463 we were (trying to) add the full transitive closure to a constraint set's BDD, and normalization gave a useful reduction in the number of nodes in a typical BDD.

Now that we don't store the transitive closure explicitly, that second reason is no longer relevant. Our sequent map can store that full transitive closure much more efficiently than the expanded BDD would have. This helps fix some false positives on #20933, where we're seeing some (incorrect, need to be fixed, but ideally not blocking this effort) assignability failures between a type and its normalization.

Normalization is still useful for display purposes, and so we do normalize the upper/lower bounds before building up our display representation of a constraint set BDD.

---

_Label `internal` added by @dcreager on 2025-11-18 19:38_

---

_Review requested from @carljm by @dcreager on 2025-11-18 19:38_

---

_Review requested from @AlexWaygood by @dcreager on 2025-11-18 19:38_

---

_Review requested from @sharkdp by @dcreager on 2025-11-18 19:38_

---

_Label `ty` added by @dcreager on 2025-11-18 19:38_

---

_Comment by @astral-sh-bot[bot] on 2025-11-18 19:40_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-18 19:41_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_@carljm approved on 2025-11-18 23:56_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/constraints.rs`:2309 on 2025-11-19 08:18_

```suggestion
        // identify constraints that are identical besides e.g. ordering of union/intersection
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/constraints.rs`:2320 on 2025-11-19 08:30_

The explanation above seems to imply that we could (should?) return early here? Because if `a → b` always implies `a ∧ b = a`, then the code below would otherwise add *two additional* `a → b` implications and one `a → a` implication, both of which seem unnecessary?

---

_@sharkdp approved on 2025-11-19 08:30_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-11-19 15:37_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:2320 on 2025-11-19 15:58_

There are other protections in place against both of those: The `single_implications` and `pair_implications` maps now have sets to store all of the consequents, so two copies of `a → b` will automatically get de-duped, no matter how they're created. And similarly, `add_single_implication` and `add_pair_implication` will skip any sequent of the form `a → a`, `a ∧ b → a`, or `a ∧ b → b`, since they are not useful.

---

_@dcreager reviewed on 2025-11-19 15:58_

---

_Merged by @dcreager on 2025-11-19 16:49_

---

_Closed by @dcreager on 2025-11-19 16:49_

---

_Branch deleted on 2025-11-19 16:49_

---
