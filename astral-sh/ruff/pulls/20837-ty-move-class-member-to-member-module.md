```yaml
number: 20837
title: "[ty] Move `class_member` to `member` module"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: david/refactor-member-module
created_at: 2025-10-13T08:19:41Z
updated_at: 2025-10-13T08:58:39Z
url: https://github.com/astral-sh/ruff/pull/20837
synced_at: 2026-01-12T15:57:10Z
```

# [ty] Move `class_member` to `member` module

---

_@sharkdp_

## Summary

Move the `class_member` function to the `member` module. This allows us to move the `member` module into the `types` module and to reduce the visibility of its contents to `pub(super)`. The drawback is that we need to make `place::place_by_id` public.

## Test Plan

Pure refactoring.

---

_Review requested from @carljm by @sharkdp on 2025-10-13 08:19_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-13 08:19_

---

_Review requested from @dcreager by @sharkdp on 2025-10-13 08:19_

---

_Label `internal` added by @sharkdp on 2025-10-13 08:19_

---

_Label `ty` added by @sharkdp on 2025-10-13 08:19_

---

_Comment by @github-actions[bot] on 2025-10-13 08:21_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-13 08:22_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@MichaReiser approved on 2025-10-13 08:24_

---

_@AlexWaygood approved on 2025-10-13 08:32_

---

_Merged by @sharkdp on 2025-10-13 08:58_

---

_Closed by @sharkdp on 2025-10-13 08:58_

---

_Branch deleted on 2025-10-13 08:58_

---
