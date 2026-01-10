```yaml
number: 14884
title: Improve mdtests style
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: rk-mdtests
created_at: 2024-12-10T00:42:38Z
updated_at: 2024-12-10T15:48:19Z
url: https://github.com/astral-sh/ruff/pull/14884
synced_at: 2026-01-10T20:42:27Z
```

# Improve mdtests style

---

_Pull request opened by @InSyncWithFoo on 2024-12-10 00:42_

## Summary

Resolves #14839.

## Test Plan

All tests pass after modifications.


---

_Review requested from @carljm by @InSyncWithFoo on 2024-12-10 00:42_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2024-12-10 00:42_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2024-12-10 00:42_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2024-12-10 00:42_

---

_Comment by @github-actions[bot] on 2024-12-10 00:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/any.md`:71 on 2024-12-10 00:54_

Throughout this PR, when a test doesn't depend on the return annotation of a function, let's reduce visual noise by omitting it.
```suggestion
def _(s: Subclass):
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/assignment/augmented.md`:161 on 2024-12-10 00:56_

Is there a reason this test wasn't rewritten similar to the other tests?

---

_@carljm reviewed on 2024-12-10 02:36_

Thank you!!

Will let @AlexWaygood take a look as well.

---

_Label `red-knot` added by @MichaReiser on 2024-12-10 07:05_

---

_@InSyncWithFoo reviewed on 2024-12-10 08:43_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/resources/mdtest/assignment/augmented.md`:161 on 2024-12-10 08:43_

Not sure why I missed that. Fixed.

---

_@AlexWaygood approved on 2024-12-10 13:02_

Thank you, this is really helpful! I pushed a couple more style nitpicks (seemed like a good opportunity). I also reverted the changes you made to `crates/red_knot_python_semantic/resources/mdtest/scopes/unbound.md`: the mdtests in this file were specifically testing the interactions of class scopes with the global scope, so moving the tests inside a function scope is unfortunately not appropriate for those snippets.

---

_Label `testing` added by @AlexWaygood on 2024-12-10 13:02_

---

_Comment by @MichaReiser on 2024-12-10 13:05_

That's a lot of removed boilerplate!

---

_Merged by @AlexWaygood on 2024-12-10 13:05_

---

_Closed by @AlexWaygood on 2024-12-10 13:05_

---

_Branch deleted on 2024-12-10 15:48_

---
