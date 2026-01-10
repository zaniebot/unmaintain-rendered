```yaml
number: 15121
title: "[red-knot] Infer value expr for empty list / tuple target"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/empty-target
created_at: 2024-12-23T10:19:27Z
updated_at: 2024-12-23T10:30:37Z
url: https://github.com/astral-sh/ruff/pull/15121
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Infer value expr for empty list / tuple target

---

_Pull request opened by @dhruvmanila on 2024-12-23 10:19_

## Summary

This PR resolves https://github.com/astral-sh/ruff/pull/15058#discussion_r1893868406 by inferring the value expression even if there are no targets in the list / tuple expression.

## Test Plan

Remove TODO from corpus tests, making sure it doesn't panic.


---

_Label `red-knot` added by @dhruvmanila on 2024-12-23 10:19_

---

_Review requested from @carljm by @dhruvmanila on 2024-12-23 10:19_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-12-23 10:19_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-12-23 10:19_

---

_Review requested from @sharkdp by @dhruvmanila on 2024-12-23 10:19_

---

_Renamed from "re[red-knot] Infer value expr for empty list / tuple target" to "[red-knot] Infer value expr for empty list / tuple target" by @dhruvmanila on 2024-12-23 10:19_

---

_Comment by @github-actions[bot] on 2024-12-23 10:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-12-23 10:26_

---

_Merged by @dhruvmanila on 2024-12-23 10:30_

---

_Closed by @dhruvmanila on 2024-12-23 10:30_

---

_Branch deleted on 2024-12-23 10:30_

---
