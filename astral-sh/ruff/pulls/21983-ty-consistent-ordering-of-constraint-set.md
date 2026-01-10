```yaml
number: 21983
title: "[ty] Consistent ordering of constraint set specializations, take 2"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/source-order-constraints
created_at: 2025-12-15T03:04:22Z
updated_at: 2025-12-15T19:24:10Z
url: https://github.com/astral-sh/ruff/pull/21983
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Consistent ordering of constraint set specializations, take 2

---

_Pull request opened by @dcreager on 2025-12-15 03:04_

In https://github.com/astral-sh/ruff/pull/21957, we tried to use `union_or_intersection_elements_ordering` to provide a stable ordering of the union and intersection elements that are created when determining which type a typevar should specialize to. @AlexWaygood [pointed out](https://github.com/astral-sh/ruff/pull/21551#discussion_r2616543762) that this won't work, since that provides a consistent ordering within a single process run, but does not provide a stable ordering across runs.

This is an attempt to produce a proper stable ordering for constraint sets, so that we end up with consistent diagnostic and test output.

We do this by maintaining a new `source_order` field on each interior BDD node, which records when that node's constraint was added to the set. Several of the BDD operators (`and`, `or`, etc) now have `_with_offset` variants, which update each `source_order` in the rhs to be larger than any of the `source_order`s in the lhs. This is what causes that field to be in line with (a) when you add each constraint to the set, and (b) the order of the parameters you provide to `and`, `or`, etc. Then we sort by that new field before constructing the union/intersection types when creating a specialization.

---

_Label `ty` added by @dcreager on 2025-12-15 03:04_

---

_Comment by @astral-sh-bot[bot] on 2025-12-15 03:06_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-15 03:08_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1218:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5123 diagnostics
+ Found 5124 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @dcreager on 2025-12-15 15:43_

---

_Review requested from @carljm by @dcreager on 2025-12-15 15:43_

---

_Review requested from @AlexWaygood by @dcreager on 2025-12-15 15:43_

---

_Review requested from @sharkdp by @dcreager on 2025-12-15 15:43_

---

_Label `internal` added by @AlexWaygood on 2025-12-15 15:49_

---

_@carljm approved on 2025-12-15 18:30_

Looks good! It's too bad how much extra work we have to do (rebuilding entire BDD subtrees) in order to preserve this ordering information, but at the moment I'm not seeing a better way.

---

_Comment by @dcreager on 2025-12-15 18:47_

> Looks good! It's too bad how much extra work we have to do (rebuilding entire BDD subtrees) in order to preserve this ordering information, but at the moment I'm not seeing a better way.

Hmm, that's a good thought. For expediency I'm just going to add a TODO, but I wonder if we can store the `other_offset` as another field on `InteriorNode` and apply it lazily whenever we walk a BDD tree.

---

_Merged by @dcreager on 2025-12-15 19:24_

---

_Closed by @dcreager on 2025-12-15 19:24_

---

_Branch deleted on 2025-12-15 19:24_

---
