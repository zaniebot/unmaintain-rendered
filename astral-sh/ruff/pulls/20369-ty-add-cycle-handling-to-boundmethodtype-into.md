```yaml
number: 20369
title: "[ty] add cycle handling to BoundMethodType::into_callable_type()"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/sympy
created_at: 2025-09-12T20:14:44Z
updated_at: 2025-09-12T20:39:40Z
url: https://github.com/astral-sh/ruff/pull/20369
synced_at: 2026-01-12T15:57:00Z
```

# [ty] add cycle handling to BoundMethodType::into_callable_type()

---

_@carljm_

## Summary

This looks like it should fix the errors that we've been seeing in sympy in recent mypy-primer runs.

## Test Plan

I wasn't able to reproduce the sympy failures locally; it looks like there is probably a dependency on the order in which files are checked. So I don't have a minimal reproducible example, and wasn't able to add a test :/ Obviously I would be happier if we could commit a regression test here, but since the change is straightforward and clearly desirable, I'm not sure how many hours it's worth trying to track it down.

Mypy-primer is still failing in CI on this PR, because it fails on the "old" ty commit already (i.e. on main). But it passes [on a no-op PR stacked on top of this](https://github.com/astral-sh/ruff/pull/20370), which strongly suggests this PR fixes the problem.

---

_Label `ty` added by @carljm on 2025-09-12 20:14_

---

_Comment by @github-actions[bot] on 2025-09-12 20:17_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Marked ready for review by @carljm on 2025-09-12 20:35_

---

_Review requested from @AlexWaygood by @carljm on 2025-09-12 20:35_

---

_Review requested from @sharkdp by @carljm on 2025-09-12 20:35_

---

_Review requested from @dcreager by @carljm on 2025-09-12 20:35_

---

_Comment by @carljm on 2025-09-12 20:37_

I'm going to go ahead and merge this to clear up CI for other PRs. Post-land review is welcome, and if anyone wants to try to track down a minimized repro so we can add a regression test, I certainly won't complain!

---

_Merged by @carljm on 2025-09-12 20:39_

---

_Closed by @carljm on 2025-09-12 20:39_

---

_Branch deleted on 2025-09-12 20:39_

---
