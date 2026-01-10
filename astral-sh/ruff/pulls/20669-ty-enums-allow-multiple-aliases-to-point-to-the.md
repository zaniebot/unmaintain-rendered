```yaml
number: 20669
title: "[ty] Enums: allow multiple aliases to point to the same member"
type: pull_request
state: merged
author: sharkdp
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: david/fix-1293
created_at: 2025-10-01T13:16:35Z
updated_at: 2025-10-01T13:51:56Z
url: https://github.com/astral-sh/ruff/pull/20669
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Enums: allow multiple aliases to point to the same member

---

_Pull request opened by @sharkdp on 2025-10-01 13:16_

## Summary

closes https://github.com/astral-sh/ty/issues/1293

## Test Plan

Regression test


---

_Label `ty` added by @sharkdp on 2025-10-01 13:16_

---

_Label `bug` added by @sharkdp on 2025-10-01 13:16_

---

_Comment by @github-actions[bot] on 2025-10-01 13:18_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-01 13:19_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @sharkdp on 2025-10-01 13:26_

---

_Review requested from @carljm by @sharkdp on 2025-10-01 13:26_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-01 13:26_

---

_Review requested from @dcreager by @sharkdp on 2025-10-01 13:26_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/enums.rs`:197 on 2025-10-01 13:33_

Shouldn't this also include `Type::BooleanLiteral(_)`? Looks like we get this wrong: https://play.ty.dev/90e8c00b-fc7b-4a5c-a051-332c5b4893ff

---

_@AlexWaygood approved on 2025-10-01 13:33_

---

_Merged by @sharkdp on 2025-10-01 13:51_

---

_Closed by @sharkdp on 2025-10-01 13:51_

---

_Branch deleted on 2025-10-01 13:51_

---
