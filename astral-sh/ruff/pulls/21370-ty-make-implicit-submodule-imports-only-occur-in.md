```yaml
number: 21370
title: "[ty] Make implicit submodule imports only occur in global scope"
type: pull_request
state: merged
author: Gankra
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: gankra/glob-only-imp
created_at: 2025-11-10T23:26:29Z
updated_at: 2025-11-10T23:59:50Z
url: https://github.com/astral-sh/ruff/pull/21370
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Make implicit submodule imports only occur in global scope

---

_Pull request opened by @Gankra on 2025-11-10 23:26_

This loses any ability to have "per-function" implicit submodule imports, to avoid the "ok but now we need per-scope imports" and "ok but this should actually introduce a global that only exists during this function" problems. A simple and clean implementation with no weird corners.

Fixes https://github.com/astral-sh/ty/issues/1482

---

_Label `ty` added by @Gankra on 2025-11-10 23:26_

---

_Review requested from @carljm by @Gankra on 2025-11-10 23:26_

---

_Review requested from @AlexWaygood by @Gankra on 2025-11-10 23:26_

---

_Review requested from @sharkdp by @Gankra on 2025-11-10 23:26_

---

_Label `ecosystem-analyzer` added by @Gankra on 2025-11-10 23:26_

---

_Review requested from @dcreager by @Gankra on 2025-11-10 23:26_

---

_Comment by @astral-sh-bot[bot] on 2025-11-10 23:28_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-10 23:29_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_@carljm approved on 2025-11-10 23:31_

---

_Comment by @astral-sh-bot[bot] on 2025-11-10 23:32_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://gankra-glob-only-imp.ecosystem-663.pages.dev/diff)** ([timing results](https://gankra-glob-only-imp.ecosystem-663.pages.dev/timing))




---

_Merged by @Gankra on 2025-11-10 23:59_

---

_Closed by @Gankra on 2025-11-10 23:59_

---

_Branch deleted on 2025-11-10 23:59_

---
