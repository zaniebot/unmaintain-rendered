```yaml
number: 20985
title: "[ty] Refactor: Use `let`-chains in a few places"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: david/ty_python_semantic-let-chains
created_at: 2025-10-20T07:45:39Z
updated_at: 2025-10-20T17:01:39Z
url: https://github.com/astral-sh/ruff/pull/20985
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Refactor: Use `let`-chains in a few places

---

_Pull request opened by @sharkdp on 2025-10-20 07:45_

## Summary

Just ran a Claude prompt I used on one of my own projects (limited to `ty_python_semantic`). I didn't add all of the proposed changes and only selected those that were unlikely to conflict with any open PRs, but those look like reasonable improvements to me.

## Test Plan

Pure refactor


---

_Label `internal` added by @sharkdp on 2025-10-20 07:45_

---

_Label `ty` added by @sharkdp on 2025-10-20 07:45_

---

_Review requested from @carljm by @sharkdp on 2025-10-20 07:45_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-20 07:45_

---

_Review requested from @dcreager by @sharkdp on 2025-10-20 07:45_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/function.rs`:620 on 2025-10-20 07:47_

It will take me a while to get used to this syntax. My brain immediately raises a syntax error after `implementation` that there's a missing `{` 😅 

---

_@MichaReiser reviewed on 2025-10-20 07:47_

---

_@MichaReiser approved on 2025-10-20 07:47_

---

_@AlexWaygood approved on 2025-10-20 08:43_

---

_Closed by @sharkdp on 2025-10-20 15:58_

---

_Reopened by @sharkdp on 2025-10-20 15:58_

---

_Comment by @github-actions[bot] on 2025-10-20 16:12_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-20 16:46_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Merged by @AlexWaygood on 2025-10-20 17:01_

---

_Closed by @AlexWaygood on 2025-10-20 17:01_

---

_Branch deleted on 2025-10-20 17:01_

---
