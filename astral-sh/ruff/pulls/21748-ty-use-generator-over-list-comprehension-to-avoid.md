```yaml
number: 21748
title: "[ty] Use generator over list comprehension to avoid cast"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/benchmarks-remove-cast
created_at: 2025-12-02T07:33:42Z
updated_at: 2025-12-02T07:47:49Z
url: https://github.com/astral-sh/ruff/pull/21748
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Use generator over list comprehension to avoid cast

---

_Pull request opened by @MichaReiser on 2025-12-02 07:33_

## Summary

Suggested by @sharkdp. ty (incorrectly) infers `list[Unknown]` when using `asyncio.gather` 
because the list comprehension is unioned with `| Unknown` (because it's a list that could be modified but never will). 

Using a generator avoids the `| Unknown`, making ty infer the correct return type.





---

_Label `internal` added by @MichaReiser on 2025-12-02 07:33_

---

_Label `ty` added by @MichaReiser on 2025-12-02 07:33_

---

_Review requested from @carljm by @MichaReiser on 2025-12-02 07:33_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-02 07:33_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-02 07:33_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-02 07:33_

---

_@sharkdp approved on 2025-12-02 07:35_

---

_Comment by @astral-sh-bot[bot] on 2025-12-02 07:45_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Merged by @MichaReiser on 2025-12-02 07:47_

---

_Closed by @MichaReiser on 2025-12-02 07:47_

---

_Branch deleted on 2025-12-02 07:47_

---
