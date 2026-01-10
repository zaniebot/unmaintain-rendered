```yaml
number: 20840
title: "[ty] Move logic for `super()` inference to a new `types::bound_super` submodule"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/move-super
created_at: 2025-10-13T11:13:29Z
updated_at: 2025-10-13T11:18:14Z
url: https://github.com/astral-sh/ruff/pull/20840
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Move logic for `super()` inference to a new `types::bound_super` submodule

---

_Pull request opened by @AlexWaygood on 2025-10-13 11:13_

## Summary

`types.rs` is huge, and a significant portion of that file is now logic dedicated to inferring `super()` calls that can be easily split into a new submodule. Let's do that.

## Test Plan

This is a pure refactor; there no additional tests


---

_Review requested from @carljm by @AlexWaygood on 2025-10-13 11:13_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-13 11:13_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-13 11:13_

---

_Label `internal` added by @AlexWaygood on 2025-10-13 11:13_

---

_Label `ty` added by @AlexWaygood on 2025-10-13 11:13_

---

_Comment by @github-actions[bot] on 2025-10-13 11:15_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-13 11:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@sharkdp approved on 2025-10-13 11:17_

---

_Merged by @AlexWaygood on 2025-10-13 11:18_

---

_Closed by @AlexWaygood on 2025-10-13 11:18_

---

_Branch deleted on 2025-10-13 11:18_

---
