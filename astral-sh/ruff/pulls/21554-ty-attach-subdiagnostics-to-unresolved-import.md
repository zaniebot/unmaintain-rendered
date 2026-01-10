```yaml
number: 21554
title: "[ty] Attach subdiagnostics to `unresolved-import` errors for relative imports as well as absolute imports"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/import-diagnostic-hints
created_at: 2025-11-21T12:24:01Z
updated_at: 2025-11-21T12:40:55Z
url: https://github.com/astral-sh/ruff/pull/21554
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Attach subdiagnostics to `unresolved-import` errors for relative imports as well as absolute imports

---

_Pull request opened by @AlexWaygood on 2025-11-21 12:24_

## Summary

If we're not able to resolve an import, we attach subdiagnostics to the error saying which search paths we looked for the import in, and linking to our settings page. However, we currently only attach these subdiagnostics if `level == 0` -- i.e., if it was an _absolute_ import that was unresolved. This doesn't make much sense; if anything, relative imports are more sensitive to the details of exactly which search paths the user has or hasn't got configured. It looks to me like this is just an accident; this PR changes things so that we attach the subdiagnostic to all `unresolved-import` diagnostics, not just absolute ones.

The diff here is easiest to review with GitHub's "hide whitespace changes" feature enabled 😄

## Test Plan

Snapshot updated


---

_Review requested from @carljm by @AlexWaygood on 2025-11-21 12:24_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-21 12:24_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-21 12:24_

---

_Label `ty` added by @AlexWaygood on 2025-11-21 12:24_

---

_Label `diagnostics` added by @AlexWaygood on 2025-11-21 12:24_

---

_Comment by @astral-sh-bot[bot] on 2025-11-21 12:26_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @zsol on 2025-11-21 12:26_

> The diff here is easiest to review with GitHub's "hide whitespace changes" feature enabled 😄

Lies! 😆 

---

_@MichaReiser approved on 2025-11-21 12:27_

---

_Comment by @astral-sh-bot[bot] on 2025-11-21 12:27_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Merged by @AlexWaygood on 2025-11-21 12:40_

---

_Closed by @AlexWaygood on 2025-11-21 12:40_

---

_Branch deleted on 2025-11-21 12:40_

---
