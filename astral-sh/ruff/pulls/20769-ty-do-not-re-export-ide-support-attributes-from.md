```yaml
number: 20769
title: "[ty] Do not re-export `ide_support` attributes from `types`"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: david/ide-support-refactor
created_at: 2025-10-08T15:27:21Z
updated_at: 2025-10-08T15:45:30Z
url: https://github.com/astral-sh/ruff/pull/20769
synced_at: 2026-01-12T15:57:09Z
```

# [ty] Do not re-export `ide_support` attributes from `types`

---

_@sharkdp_

## Summary

The `types` module currently re-exports a lot of functions and data types from `types::ide_support`. One of these is called `Member`, a name that is overloaded several times already. And I'd like to add one more `Member` struct soon. Making the whole `ide_support` module public seems cleaner to me, anyway.

## Test Plan

Pure refactoring.

---

_Review requested from @carljm by @sharkdp on 2025-10-08 15:27_

---

_Label `internal` added by @sharkdp on 2025-10-08 15:27_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-08 15:27_

---

_Review requested from @dcreager by @sharkdp on 2025-10-08 15:27_

---

_Review requested from @MichaReiser by @sharkdp on 2025-10-08 15:27_

---

_Label `ty` added by @sharkdp on 2025-10-08 15:27_

---

_Renamed from "[ty] Encapsulate `ide_support` module" to "[ty] Do not re-export `ide_support` attributes from `types`" by @sharkdp on 2025-10-08 15:28_

---

_Comment by @github-actions[bot] on 2025-10-08 15:29_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_@AlexWaygood approved on 2025-10-08 15:30_

---

_Comment by @github-actions[bot] on 2025-10-08 15:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- Found 52 diagnostics
+ Found 53 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Merged by @sharkdp on 2025-10-08 15:45_

---

_Closed by @sharkdp on 2025-10-08 15:45_

---

_Branch deleted on 2025-10-08 15:45_

---
