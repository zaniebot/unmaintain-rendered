```yaml
number: 14510
title: "[red-knot] Fix: Infer type for typing.Union[..] tuple expression"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-union-type-panic
created_at: 2024-11-21T10:39:38Z
updated_at: 2024-11-21T13:09:18Z
url: https://github.com/astral-sh/ruff/pull/14510
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Fix: Infer type for typing.Union[..] tuple expression

---

_Pull request opened by @sharkdp on 2024-11-21 10:39_

## Summary

Fixes a panic related to sub-expressions of `typing.Union` where we fail to store a type for the `int, str` tuple-expression in code like this:
```py
x: Union[int, str] = 1
```

relates to [my comment](https://github.com/astral-sh/ruff/pull/14499#discussion_r1851794467) on #14499.

## Test Plan

New corpus test

---

_Review requested from @carljm by @sharkdp on 2024-11-21 10:39_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-21 10:39_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-21 10:39_

---

_Comment by @github-actions[bot] on 2024-11-21 10:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @sharkdp on 2024-11-21 10:49_

---

_Closed by @sharkdp on 2024-11-21 10:49_

---

_Branch deleted on 2024-11-21 10:49_

---

_Label `red-knot` added by @sharkdp on 2024-11-21 13:09_

---
