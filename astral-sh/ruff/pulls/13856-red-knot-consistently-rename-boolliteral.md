```yaml
number: 13856
title: "[red-knot] Consistently rename BoolLiteral => BooleanLiteral"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/bool-literal-rename
created_at: 2024-10-21T11:38:44Z
updated_at: 2024-10-22T20:14:32Z
url: https://github.com/astral-sh/ruff/pull/13856
synced_at: 2026-01-10T20:59:37Z
```

# [red-knot] Consistently rename BoolLiteral => BooleanLiteral

---

_Pull request opened by @sharkdp on 2024-10-21 11:38_

## Summary

- Consistent naming: `BoolLiteral` => `BooleanLiteral` (it's mainly the `Ty::BoolLiteral` variant that was renamed)

  I tripped over this a few times now, so I thought I'll smooth it out.
- Add a new test case for `Literal[True] <: bool`, as suggested here: https://github.com/astral-sh/ruff/pull/13781#discussion_r1804922827


---

_Review requested from @carljm by @sharkdp on 2024-10-21 11:38_

---

_Review requested from @MichaReiser by @sharkdp on 2024-10-21 11:38_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-10-21 11:38_

---

_@AlexWaygood approved on 2024-10-21 11:40_

---

_Comment by @github-actions[bot] on 2024-10-21 11:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @sharkdp on 2024-10-21 11:55_

---

_Closed by @sharkdp on 2024-10-21 11:55_

---

_Branch deleted on 2024-10-21 11:55_

---

_Label `red-knot` added by @sharkdp on 2024-10-22 20:14_

---
