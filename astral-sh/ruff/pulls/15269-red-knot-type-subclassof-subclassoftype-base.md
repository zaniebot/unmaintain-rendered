```yaml
number: 15269
title: "[red-knot] `Type::SubclassOf(SubclassOfType { base: ClassBase::Unknown }).to_instance()` should be `Unknown`, not `Any`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/subclass-of-to-instance
created_at: 2025-01-05T15:08:05Z
updated_at: 2025-01-05T15:15:55Z
url: https://github.com/astral-sh/ruff/pull/15269
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] `Type::SubclassOf(SubclassOfType { base: ClassBase::Unknown }).to_instance()` should be `Unknown`, not `Any`

---

_Pull request opened by @AlexWaygood on 2025-01-05 15:08_

## Summary

I noticed this micro-bug while working on something else.

## Test Plan

I haven't added a test here because:
- I don't know if there's currently a way to reproduce the bug from Python, so I'm not sure it's possible to write an mdtest for this
- I could add a unit test, but I'm not sure it would really be helpful. The logic being modified here is fairly trivial; adding a unit test just feels like it would make this code harder to refactor, if we ever did want to refactor the structure of `SubclassOfType`


---

_Label `red-knot` added by @AlexWaygood on 2025-01-05 15:08_

---

_Review requested from @carljm by @AlexWaygood on 2025-01-05 15:08_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-05 15:08_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-01-05 15:08_

---

_Merged by @AlexWaygood on 2025-01-05 15:14_

---

_Closed by @AlexWaygood on 2025-01-05 15:14_

---

_Branch deleted on 2025-01-05 15:14_

---

_Comment by @github-actions[bot] on 2025-01-05 15:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
