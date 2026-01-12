```yaml
number: 18218
title: "[ty] Integer indexing into `bytes` returns `int`"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: ty-bytes-subscription
created_at: 2025-05-20T14:38:24Z
updated_at: 2025-05-20T14:44:42Z
url: https://github.com/astral-sh/ruff/pull/18218
synced_at: 2026-01-12T15:56:15Z
```

# [ty] Integer indexing into `bytes` returns `int`

---

_@InSyncWithFoo_

## Summary

Resolves [#461](https://github.com/astral-sh/ty/issues/461).

ty was hardcoded to infer `BytesLiteral` types for integer indexing into `BytesLiteral`. It will now infer `IntLiteral` types instead.

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2025-05-20 14:38_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-05-20 14:38_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-05-20 14:38_

---

_Review requested from @dcreager by @InSyncWithFoo on 2025-05-20 14:38_

---

_Label `ty` added by @MichaReiser on 2025-05-20 14:40_

---

_Comment by @github-actions[bot] on 2025-05-20 14:41_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@sharkdp approved on 2025-05-20 14:41_

Thank you very much!

---

_Merged by @sharkdp on 2025-05-20 14:44_

---

_Closed by @sharkdp on 2025-05-20 14:44_

---

_Branch deleted on 2025-05-20 14:44_

---
