```yaml
number: 17750
title: "[ty] Strict validation of protocol members"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/protocol-member-validation
created_at: 2025-04-30T21:57:48Z
updated_at: 2025-08-19T22:46:04Z
url: https://github.com/astral-sh/ruff/pull/17750
synced_at: 2026-01-12T15:56:05Z
```

# [ty] Strict validation of protocol members

---

_@AlexWaygood_

## Summary

Emit warning-level diagnostics for symbols that are defined in protocol classes but not explicitly declared as protocol members. This leads to an ambiguous protocol interface that may result in ty misinterpreting the user's intent.

The mypy_primer output provides strong evidence that this is a useful lint: the protocols in ddtrace use Python 2 type comments for their members, which ty won't understand properly.

## Test Plan

Snapshots and mdtests


---

_Label `red-knot` added by @AlexWaygood on 2025-04-30 21:57_

---

_Comment by @github-actions[bot] on 2025-04-30 22:00_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
operator (https://github.com/canonical/operator)
+ ops/framework.py:61:5: warning[ambiguous-protocol-member] Cannot assign to undeclared variable in the body of a protocol class: Consider adding an annotation, e.g. `handle_kind: str = ...`
+ ops/pebble.py:231:5: warning[ambiguous-protocol-member] Cannot assign to undeclared variable in the body of a protocol class: Consider adding an annotation, e.g. `name: str = ...`
- Found 97 diagnostics
+ Found 99 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ ddtrace/internal/wrapping/__init__.py:30:5: warning[ambiguous-protocol-member] Cannot assign to undeclared variable in the body of a protocol class: Consider adding an annotation for `__dd_wrapped__`
+ ddtrace/internal/wrapping/__init__.py:31:5: warning[ambiguous-protocol-member] Cannot assign to undeclared variable in the body of a protocol class: Consider adding an annotation for `__dd_wrappers__`
+ ddtrace/internal/wrapping/context.py:417:5: warning[ambiguous-protocol-member] Cannot assign to undeclared variable in the body of a protocol class: Consider adding an annotation for `__dd_context_wrapped__`
- Found 6493 diagnostics
+ Found 6496 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-04-30 22:06_

This should probably be a different error code than `INVALID_PROTOCOL`, actually. Maybe `INVALID_PROTOCOL_MEMBER`. It deserves its own documentation, and it's pretty different from the other `INVALID_PROTOCOL` diagnostics in that it doesn't actually result in a runtime error.

---

_Renamed from "[red-knot] Strict validation of protocol members" to "[ty] Strict validation of protocol members" by @AlexWaygood on 2025-06-04 18:41_

---

_Comment by @github-actions[bot] on 2025-08-17 12:46_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @AlexWaygood on 2025-08-17 12:47_

I put this on hold for a while because I was hopeful that the machinery needed for go-to definition would make creating these diagnostics less fiddly, but it doesn't actually help there ultimately.

---

_Marked ready for review by @AlexWaygood on 2025-08-17 13:38_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-17 13:38_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-17 13:38_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-17 13:38_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-08-17 13:38_

---

_@carljm approved on 2025-08-19 22:27_

Looks good, thank you!

---

_Merged by @AlexWaygood on 2025-08-19 22:45_

---

_Closed by @AlexWaygood on 2025-08-19 22:45_

---

_Branch deleted on 2025-08-19 22:45_

---
