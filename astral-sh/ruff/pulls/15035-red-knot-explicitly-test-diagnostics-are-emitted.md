```yaml
number: 15035
title: "[red-knot] Explicitly test diagnostics are emitted for unresolvable submodule imports"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/unresolvable-submodule-errors
created_at: 2024-12-17T12:51:10Z
updated_at: 2024-12-17T17:29:23Z
url: https://github.com/astral-sh/ruff/pull/15035
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Explicitly test diagnostics are emitted for unresolvable submodule imports

---

_Pull request opened by @AlexWaygood on 2024-12-17 12:51_

Everything's working as expected here, but we didn't have explicit tests for these; this PR just adds some missing cases

---

_Review requested from @carljm by @AlexWaygood on 2024-12-17 12:51_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-12-17 12:51_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-12-17 12:51_

---

_Label `red-knot` added by @AlexWaygood on 2024-12-17 12:51_

---

_Merged by @AlexWaygood on 2024-12-17 12:55_

---

_Closed by @AlexWaygood on 2024-12-17 12:55_

---

_Branch deleted on 2024-12-17 12:56_

---

_Comment by @github-actions[bot] on 2024-12-17 12:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `testing` added by @AlexWaygood on 2024-12-17 12:59_

---

_@T-256 reviewed on 2024-12-17 17:27_

---

_Review comment by @T-256 on `crates/red_knot_python_semantic/resources/mdtest/import/basic.md`:102 on 2024-12-17 17:27_

IMO, this message could mention only `b`. it'd improve discoverability when debugging.

---

_@AlexWaygood reviewed on 2024-12-17 17:29_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/basic.md`:102 on 2024-12-17 17:29_

Yeah, there's definitely room for improvement here. I mainly wanted to add a test that made sure that we emitted a diagnostic on both of these (because I nearly proposed a refactor that would have accidentally made that not-the-case, and no tests failed on my bad patch)

---
