```yaml
number: 21285
title: "[ty] Favour imported symbols over builtin symbols"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: favour-imported-over-builtin
created_at: 2025-11-05T19:17:44Z
updated_at: 2025-11-06T11:48:48Z
url: https://github.com/astral-sh/ruff/pull/21285
synced_at: 2026-01-12T15:57:20Z
```

# [ty] Favour imported symbols over builtin symbols

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

Raised by @AlexWaygood.

We previously did not favour imported symbols, when we probably should've 

## Test Plan

Add test showing that we favour imported symbol even if it is alphabetically after other symbols that are builtin.


---

_Review requested from @carljm by @MatthewMckee4 on 2025-11-05 19:17_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-11-05 19:17_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-11-05 19:17_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-11-05 19:17_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-11-05 19:17_

---

_Renamed from "Favour imported symbols over builtin symbols" to "[ty] Favour imported symbols over builtin symbols" by @MatthewMckee4 on 2025-11-05 19:19_

---

_Comment by @github-actions[bot] on 2025-11-05 19:20_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-11-05 19:21_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `server` added by @AlexWaygood on 2025-11-05 19:46_

---

_Label `ty` added by @AlexWaygood on 2025-11-05 19:46_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-11-05 22:16_

---

_Review request for @dcreager removed by @MichaReiser on 2025-11-05 22:16_

---

_Review request for @carljm removed by @MichaReiser on 2025-11-05 22:16_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-11-05 22:16_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-11-05 22:16_

---

_@BurntSushi approved on 2025-11-06 11:46_

Nice!

---

_Merged by @BurntSushi on 2025-11-06 11:46_

---

_Closed by @BurntSushi on 2025-11-06 11:46_

---

_Branch deleted on 2025-11-06 11:48_

---
