```yaml
number: 14961
title: "[red-knot] Use `type[Unknown]` rather than `Unknown` as the fallback metaclass for invalid classes"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/type-unknown
created_at: 2024-12-13T19:44:01Z
updated_at: 2024-12-13T19:50:26Z
url: https://github.com/astral-sh/ruff/pull/14961
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Use `type[Unknown]` rather than `Unknown` as the fallback metaclass for invalid classes

---

_Pull request opened by @AlexWaygood on 2024-12-13 19:44_

## Summary

This PR changes things so that we now infer `type[Unknown]` rather than `Unknown` as the fallback metaclass for an invalid class (either one that has a cyclic definition, or one where the metaclasses of the bases conflict). This better reflects the invariant that a class's metaclass must _always_ be an instance of `type`. It follows the same principles established in https://github.com/astral-sh/ruff/pull/14942.

## Test Plan

Assertions in existing mdtests updated. No new mdtests added, since the existing ones are reasonably comprehensive.


---

_Label `red-knot` added by @AlexWaygood on 2024-12-13 19:44_

---

_Review requested from @carljm by @AlexWaygood on 2024-12-13 19:44_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-12-13 19:44_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-12-13 19:44_

---

_@carljm approved on 2024-12-13 19:47_

---

_Merged by @AlexWaygood on 2024-12-13 19:48_

---

_Closed by @AlexWaygood on 2024-12-13 19:48_

---

_Branch deleted on 2024-12-13 19:48_

---

_Comment by @github-actions[bot] on 2024-12-13 19:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
