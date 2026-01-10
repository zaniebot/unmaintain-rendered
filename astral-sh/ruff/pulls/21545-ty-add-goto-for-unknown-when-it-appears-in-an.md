```yaml
number: 21545
title: "[ty] Add goto for `Unknown` when it appears in an inlay hint"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: alex/unknown-goto-type
created_at: 2025-11-20T18:39:39Z
updated_at: 2025-11-20T18:55:15Z
url: https://github.com/astral-sh/ruff/pull/21545
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Add goto for `Unknown` when it appears in an inlay hint

---

_Pull request opened by @AlexWaygood on 2025-11-20 18:39_

## Summary

Allow users to double-click on `Unknown` in inlay hints and jump to the symbol definition in `ty_extensions`.

This isn't particularly useful right now because we just have `Unknown = object()` in `ty_extensions`, but we could potentially add some documentation to the `Unknown` definition there that explains that it's `Any`-like etc.

## Test Plan

Snapshots updated


---

_Review requested from @Gankra by @AlexWaygood on 2025-11-20 18:39_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-20 18:39_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-20 18:39_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-20 18:39_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-11-20 18:39_

---

_Label `server` added by @AlexWaygood on 2025-11-20 18:39_

---

_Label `ty` added by @AlexWaygood on 2025-11-20 18:39_

---

_Comment by @astral-sh-bot[bot] on 2025-11-20 18:41_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-20 18:43_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_@Gankra approved on 2025-11-20 18:53_

---

_Merged by @AlexWaygood on 2025-11-20 18:55_

---

_Closed by @AlexWaygood on 2025-11-20 18:55_

---

_Branch deleted on 2025-11-20 18:55_

---
