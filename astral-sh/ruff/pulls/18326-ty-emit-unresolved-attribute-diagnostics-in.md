```yaml
number: 18326
title: "[ty] Emit unresolved-attribute diagnostics in annotated assignments"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
assignees: []
draft: true
base: main
head: david/fix-509
created_at: 2025-05-26T17:42:17Z
updated_at: 2025-05-28T08:45:19Z
url: https://github.com/astral-sh/ruff/pull/18326
synced_at: 2026-01-12T15:56:16Z
```

# [ty] Emit unresolved-attribute diagnostics in annotated assignments

---

_@sharkdp_

## Summary

Does not yet include any decision on whether or not these annotated attribute assignments should be allowed (for non-`self` assignments), but makes sure that we emit `unresolved-attribute` errors when trying to write to a non-existent attribute.

closes astral-sh/ty#509

## Test Plan

New Markdown tests


---

_Review requested from @carljm by @sharkdp on 2025-05-26 17:42_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-26 17:42_

---

_Review requested from @dcreager by @sharkdp on 2025-05-26 17:42_

---

_Label `ty` added by @sharkdp on 2025-05-26 17:42_

---

_Comment by @github-actions[bot] on 2025-05-26 17:45_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Converted to draft by @sharkdp on 2025-05-26 17:57_

---

_Comment by @carljm on 2025-05-27 16:54_

FWIW, I think annotated assignments to attribute (or subscript) expressions should simply always be an error. It's never sensible to declare a type for a name in some other scope. AFAIK this is always an error for other type checkers.

---

_Closed by @sharkdp on 2025-05-28 08:45_

---
