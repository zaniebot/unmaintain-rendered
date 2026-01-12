```yaml
number: 20044
title: Fix incorrect lsp inlay hint type
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: inlay-hint-type
created_at: 2025-08-22T14:20:25Z
updated_at: 2025-11-06T11:48:21Z
url: https://github.com/astral-sh/ruff/pull/20044
synced_at: 2026-01-12T15:56:53Z
```

# Fix incorrect lsp inlay hint type

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Previously we just communicated that the inlay hint type was "TYPE", so now we give the correct type



---

_Marked ready for review by @MatthewMckee4 on 2025-08-22 14:21_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-08-22 14:21_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-08-22 14:21_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-08-22 14:21_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-08-22 14:21_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-08-22 14:21_

---

_Comment by @github-actions[bot] on 2025-08-22 14:22_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Label `server` added by @AlexWaygood on 2025-08-22 14:22_

---

_Label `ty` added by @AlexWaygood on 2025-08-22 14:22_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/inlay_hints.rs`:74 on 2025-08-22 14:22_

Nice! Would you mind to also wire it up in wasm and the playground?

https://github.com/astral-sh/ruff/blob/859475f017c3295ba2dbac144dcefdb2a2318250/crates/ty_wasm/src/lib.rs#L980-L987

and here

https://github.com/astral-sh/ruff/blob/859475f017c3295ba2dbac144dcefdb2a2318250/playground/ty/src/Editor/Editor.tsx#L410-L416

---

_@MichaReiser reviewed on 2025-08-22 14:22_

Nice!

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-08-22 14:22_

---

_@MatthewMckee4 reviewed on 2025-08-22 14:23_

---

_Review comment by @MatthewMckee4 on `crates/ty_server/src/server/api/requests/inlay_hints.rs`:74 on 2025-08-22 14:23_

yeah

---

_Comment by @github-actions[bot] on 2025-08-22 14:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @MatthewMckee4 on 2025-08-22 14:40_

Not really sure how the ide handles these kinds differently

---

_@MichaReiser approved on 2025-08-22 15:12_

Thank you

---

_Merged by @MichaReiser on 2025-08-22 15:12_

---

_Closed by @MichaReiser on 2025-08-22 15:12_

---

_Branch deleted on 2025-11-06 11:48_

---
