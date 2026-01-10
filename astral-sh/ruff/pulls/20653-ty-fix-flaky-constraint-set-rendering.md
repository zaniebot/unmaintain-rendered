```yaml
number: 20653
title: "[ty] Fix flaky constraint set rendering"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/fix-flaky-constraints
created_at: 2025-09-30T20:16:58Z
updated_at: 2025-10-01T07:14:37Z
url: https://github.com/astral-sh/ruff/pull/20653
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Fix flaky constraint set rendering

---

_Pull request opened by @dcreager on 2025-09-30 20:16_

This doesn't seem to be flaky in the sense of tests failing non-deterministically, but they are flaky in the sense of unrelated changes causing testing failures from the clauses of a constraint set being rendered in different orders. This flakiness is because we're using Salsa IDs to determine the order in which typevars appear in a constraint set BDD, and those IDs are assigned non-deterministically.

The fix is ham-fisted but effective: sort the constraints in each clause, and the clauses in each set, as part of the rendering process. Constraint sets are only rendered in our test cases, so we don't need to over-optimize this.

---

_Label `internal` added by @dcreager on 2025-09-30 20:16_

---

_Label `ty` added by @dcreager on 2025-09-30 20:16_

---

_Review requested from @carljm by @dcreager on 2025-09-30 20:17_

---

_Review requested from @AlexWaygood by @dcreager on 2025-09-30 20:17_

---

_Review requested from @sharkdp by @dcreager on 2025-09-30 20:17_

---

_Comment by @github-actions[bot] on 2025-09-30 20:18_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-30 20:19_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@ibraheemdev approved on 2025-09-30 20:31_

---

_Comment by @sharkdp on 2025-10-01 07:14_

Merging this, as I'm seeing it on some other PRs too.

---

_Merged by @sharkdp on 2025-10-01 07:14_

---

_Closed by @sharkdp on 2025-10-01 07:14_

---

_Branch deleted on 2025-10-01 07:14_

---
