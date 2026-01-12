```yaml
number: 21127
title: "[ty] Dont provide goto definition for definitions which are not reexported in builtins"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: fix-not-reexported-builtin-goto
created_at: 2025-10-29T18:13:56Z
updated_at: 2025-11-06T11:48:55Z
url: https://github.com/astral-sh/ruff/pull/21127
synced_at: 2026-01-12T15:57:16Z
```

# [ty] Dont provide goto definition for definitions which are not reexported in builtins

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

Resolves https://github.com/astral-sh/ty/issues/1451

## Test Plan

Add test that previously fails that shows that imports in builtins that are not reexported are not given goto definitions.


---

_Review requested from @carljm by @MatthewMckee4 on 2025-10-29 18:13_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-10-29 18:13_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-10-29 18:13_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-10-29 18:13_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-10-29 18:13_

---

_Renamed from "Dont supply goto definition for definitions which are not reexported in builtins" to "[ty] Dont provide goto definition for definitions which are not reexported in builtins" by @MatthewMckee4 on 2025-10-29 18:15_

---

_Comment by @github-actions[bot] on 2025-10-29 18:18_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-29 18:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `server` added by @AlexWaygood on 2025-10-29 18:25_

---

_Label `ty` added by @AlexWaygood on 2025-10-29 18:25_

---

_@MichaReiser approved on 2025-10-29 18:27_

Nice, I didn't know we already have a method to test exactly this

---

_Comment by @MatthewMckee4 on 2025-10-29 18:31_

Took me a while to find 😃 

---

_Merged by @MichaReiser on 2025-10-29 18:39_

---

_Closed by @MichaReiser on 2025-10-29 18:39_

---

_Branch deleted on 2025-11-06 11:48_

---
