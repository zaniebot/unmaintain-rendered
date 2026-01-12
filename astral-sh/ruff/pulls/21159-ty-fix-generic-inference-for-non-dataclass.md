```yaml
number: 21159
title: "[ty] Fix generic inference for non-dataclass inheriting from generic dataclass"
type: pull_request
state: merged
author: saada
labels:
  - ty
assignees: []
merged: true
base: main
head: fix-dataclass-generic-inheritance-1427
created_at: 2025-10-31T03:26:31Z
updated_at: 2025-10-31T12:55:19Z
url: https://github.com/astral-sh/ruff/pull/21159
synced_at: 2026-01-12T15:57:17Z
```

# [ty] Fix generic inference for non-dataclass inheriting from generic dataclass

---

_@saada_

## Summary

Fixes https://github.com/astral-sh/ty/issues/1427

This PR fixes a regression introduced in alpha.24 where non-dataclass children of generic dataclasses lost generic type parameter information during `__init__` synthesis.

The issue occurred because when looking up inherited members in the MRO, the child class's `inherited_generic_context` was correctly passed down, but `own_synthesized_member()` (which synthesizes dataclass `__init__` methods) didn't accept this parameter. It only used `self.inherited_generic_context(db)`, which returned the parent's context instead of the child's.

The fix threads the child's generic context through to the synthesis logic, allowing proper generic type inference for inherited dataclass constructors.

## Test Plan

- Added regression test for non-dataclass inheriting from generic dataclass
- Verified the exact repro case from the issue now works
- All 277 mdtest tests passing
- Clippy clean
- Manually verified with Python runtime, mypy, and pyright - all accept this code pattern

## Verification

Tested against multiple type checkers:
- ✅ Python runtime: Code works correctly
- ✅ mypy: No issues found
- ✅ pyright: 0 errors, 0 warnings
- ✅ ty alpha.23: Worked (before regression)
- ❌ ty alpha.24: Regression
- ✅ ty with this fix: Works correctly

---

_Review requested from @carljm by @saada on 2025-10-31 03:26_

---

_Review requested from @AlexWaygood by @saada on 2025-10-31 03:26_

---

_Review requested from @sharkdp by @saada on 2025-10-31 03:26_

---

_Review requested from @dcreager by @saada on 2025-10-31 03:26_

---

_Comment by @github-actions[bot] on 2025-10-31 10:21_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-31 10:22_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `ty` added by @AlexWaygood on 2025-10-31 10:55_

---

_@sharkdp reviewed on 2025-10-31 10:57_

Thank you for working on this. It looks like mdformat broke the Markdown tests. This can happen if the line becomes too long. You can put the `# revealed: …` assertion in a separate line above.

---

_@sharkdp reviewed on 2025-10-31 12:54_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclasses.md`:861 on 2025-10-31 12:54_

Thank you for highlighting this. I opened https://github.com/astral-sh/ty/issues/1461 to track this.

---

_@sharkdp approved on 2025-10-31 12:55_

Thank you!

---

_Merged by @sharkdp on 2025-10-31 12:55_

---

_Closed by @sharkdp on 2025-10-31 12:55_

---
