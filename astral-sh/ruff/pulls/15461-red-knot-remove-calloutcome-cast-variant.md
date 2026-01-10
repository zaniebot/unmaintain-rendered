```yaml
number: 15461
title: "[red-knot] remove CallOutcome::Cast variant"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/cast-simplification
created_at: 2025-01-13T18:53:07Z
updated_at: 2025-01-13T18:59:24Z
url: https://github.com/astral-sh/ruff/pull/15461
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] remove CallOutcome::Cast variant

---

_Pull request opened by @carljm on 2025-01-13 18:53_

## Summary

Simplification follow-up to #15413.

There's no need to have a dedicated `CallOutcome` variant for every known function, it's only necessary if the special-cased behavior of the known function includes emitting extra diagnostics. For `typing.cast`, there's no such need; we can use the regular `Callable` outcome variant, and update the return type according to the cast. (This is the same way we already handle `len`.)

One reason to avoid proliferating unnecessary `CallOutcome` variants is that currently we have to explicitly add emitting call-binding diagnostics, for each outcome variant. So we were previously wrongly silencing any binding diagnostics on calls to `typing.cast`. Fixing this revealed a separate bug, that we were emitting a bogus error anytime more than one keyword argument mapped to a `**kwargs` parameter. So this PR also adds test and fix for that bug.

## Test Plan

Existing `cast` tests pass unchanged, added new test for `**kwargs` bug.

---

_Review requested from @MichaReiser by @carljm on 2025-01-13 18:53_

---

_Review requested from @AlexWaygood by @carljm on 2025-01-13 18:53_

---

_Review requested from @sharkdp by @carljm on 2025-01-13 18:53_

---

_@AlexWaygood approved on 2025-01-13 18:56_

Thanks. Sorry for missing that when reviewing

---

_Label `red-knot` added by @AlexWaygood on 2025-01-13 18:56_

---

_Comment by @carljm on 2025-01-13 18:58_

No worries! Both approaches are reasonable, and the issue of missing binding diagnostics is very subtle, and due to the fact that binding diagnostics are integrated in a very hacky way right now. Looking forward to your outcomes refactor hopefully improving that!

---

_Merged by @carljm on 2025-01-13 18:58_

---

_Closed by @carljm on 2025-01-13 18:58_

---

_Branch deleted on 2025-01-13 18:58_

---

_Comment by @github-actions[bot] on 2025-01-13 18:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
