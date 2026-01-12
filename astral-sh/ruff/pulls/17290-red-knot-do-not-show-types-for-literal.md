```yaml
number: 17290
title: "[red-knot] Do not show types for literal expressions on hover"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-hover
created_at: 2025-04-08T01:13:52Z
updated_at: 2025-04-08T10:21:28Z
url: https://github.com/astral-sh/ruff/pull/17290
synced_at: 2026-01-12T15:56:01Z
```

# [red-knot] Do not show types for literal expressions on hover

---

_@InSyncWithFoo_

## Summary

Resolves #17289.

After this change, Red Knot will no longer show types on hover for `None`, `...`, `True`, `False`, numbers, strings (but not f-strings), and bytes literals.

## Test Plan

Unit tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2025-04-08 01:13_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2025-04-08 01:13_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-04-08 01:13_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-04-08 01:13_

---

_Review requested from @dcreager by @InSyncWithFoo on 2025-04-08 01:13_

---

_Comment by @InSyncWithFoo on 2025-04-08 01:16_

(Props to @MichaReiser for the ergonomic test framework.)

---

_Comment by @github-actions[bot] on 2025-04-08 01:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-04-08 01:23_

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

_Comment by @MichaReiser on 2025-04-08 06:22_

Thanks for the kind words. I'm still not fully convinced we want to hide the types for all literals. I do agree that we should hide them for docstrings. Interested to hear some more opinions.

---

_Label `red-knot` added by @MichaReiser on 2025-04-08 06:22_

---

_Comment by @MichaReiser on 2025-04-08 06:25_

I put this back in draft so that we can have the discussion in one place

---

_Converted to draft by @MichaReiser on 2025-04-08 06:25_

---

_Marked ready for review by @MichaReiser on 2025-04-08 07:05_

---

_Merged by @MichaReiser on 2025-04-08 07:05_

---

_Closed by @MichaReiser on 2025-04-08 07:05_

---

_Branch deleted on 2025-04-08 10:21_

---
