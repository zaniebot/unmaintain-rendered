```yaml
number: 14750
title: "[red-knot] `is_subtype_of` fix for `KnownInstance` types"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-14731
created_at: 2024-12-03T08:55:41Z
updated_at: 2024-12-03T11:03:29Z
url: https://github.com/astral-sh/ruff/pull/14750
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] `is_subtype_of` fix for `KnownInstance` types

---

_Pull request opened by @sharkdp on 2024-12-03 08:55_

## Summary

`KnownInstance::instance_fallback` may return instances of supertypes. For example, it returns an instance of `_SpecialForm` for `Literal`. This means it can't be used on the right-hand side of `is_subtype_of` relationships, because it might lead to false positives.

I can lead to false negatives on the left hand side of `is_subtype_of`, but this is at least a known limitation. False negatives are fine for most applications, but false positives can lead to wrong results in intersection-simplification, for example.

closes #14731

## Test Plan

Added regression test

---

_Label `red-knot` added by @sharkdp on 2024-12-03 08:55_

---

_Review requested from @carljm by @sharkdp on 2024-12-03 08:55_

---

_Review requested from @MichaReiser by @sharkdp on 2024-12-03 08:55_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-12-03 08:55_

---

_Comment by @github-actions[bot] on 2024-12-03 09:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-12-03 10:21_

---

_Merged by @sharkdp on 2024-12-03 11:03_

---

_Closed by @sharkdp on 2024-12-03 11:03_

---

_Branch deleted on 2024-12-03 11:03_

---
