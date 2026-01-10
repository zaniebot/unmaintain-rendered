```yaml
number: 16915
title: "[red-knot] fix ordering of `ClassDef` semantic index building"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: fix-class-registration
created_at: 2025-03-22T14:33:10Z
updated_at: 2025-03-24T02:14:50Z
url: https://github.com/astral-sh/ruff/pull/16915
synced_at: 2026-01-10T19:40:36Z
```

# [red-knot] fix ordering of `ClassDef` semantic index building

---

_Pull request opened by @mtshiba on 2025-03-22 14:33_

## Summary

From #16861

This PR fixes the incorrect `ClassDef` handling of `SemanticIndexBuilder::visit_stmt`, which fixes some of the incorrect behavior of referencing the class itself in the class scope (a complete fix requires a different fix, which will be done in the another PR).



---

_Review requested from @carljm by @mtshiba on 2025-03-22 14:33_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-03-22 14:33_

---

_Review requested from @sharkdp by @mtshiba on 2025-03-22 14:33_

---

_Review requested from @dcreager by @mtshiba on 2025-03-22 14:33_

---

_Comment by @github-actions[bot] on 2025-03-22 14:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `red-knot` added by @MichaReiser on 2025-03-22 16:45_

---

_Comment by @MichaReiser on 2025-03-22 16:46_

It seems that https://github.com/astral-sh/ruff/pull/16916/ contains the same changes. Is this intentional? Are those PRs stacked?

---

_Comment by @mtshiba on 2025-03-23 04:38_

I separated the PRs because we needed to fix two different parts for the same issue.
I thought PR #16916 was dependent on this PR, but now I see that the test itself can pass on its own. Thus, the inclusion of this PR's commit in that PR's commits, while intentional, does not seem necessary.

---

_@carljm reviewed on 2025-03-23 13:19_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/classes.md`:187 on 2025-03-23 13:19_

Not really related to this PR, but the non-subscriptable error here is wrong, and now that we remove the one TODO we should probably clarify that.
```suggestion
# TODO: the unresolved-reference error is correct, the non-subscriptable is not
# error: [non-subscriptable]
# error: [unresolved-reference]
```

---

_Merged by @carljm on 2025-03-23 13:23_

---

_Closed by @carljm on 2025-03-23 13:23_

---

_Branch deleted on 2025-03-24 02:14_

---
