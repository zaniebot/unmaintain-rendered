```yaml
number: 16725
title: "[red-knot] fix: improve type inference for binary ops on tuples"
type: pull_request
state: merged
author: cake-monotone
labels:
  - ty
assignees: []
merged: true
base: main
head: red-knot-handle-infer-special-binary-op-on-tuples
created_at: 2025-03-14T09:05:23Z
updated_at: 2025-03-14T11:29:57Z
url: https://github.com/astral-sh/ruff/pull/16725
synced_at: 2026-01-12T15:55:56Z
```

# [red-knot] fix: improve type inference for binary ops on tuples

---

_@cake-monotone_

## Summary

This PR includes minor improvements to binary operation inference, specifically for tuple concatenation.

### Before

```py
reveal_type((1, 2) + (3, 4))  # revealed: @Todo(return type of decorated function)
# If TODO is ignored, the revealed type would be `tuple[1|2|3|4, ...]`
```

The `builtins.tuple` type stub defines `__add__`, but it appears to only work for homogeneous tuples. However, I think this limitation is not ideal for many use cases.

### After

```py
reveal_type((1, 2) + (3, 4))  # revealed: tuple[Literal[1], Literal[2], Literal[3], Literal[4]]
```

## Test Plan

### Added
- `mdtest/binary/tuples.md`

### Affected
- `mdtest/slots.md` (a test have been moved out of the `False-Negative` block.)



---

_Review requested from @carljm by @cake-monotone on 2025-03-14 09:05_

---

_Review requested from @MichaReiser by @cake-monotone on 2025-03-14 09:05_

---

_Review requested from @AlexWaygood by @cake-monotone on 2025-03-14 09:05_

---

_Review requested from @sharkdp by @cake-monotone on 2025-03-14 09:05_

---

_Comment by @github-actions[bot] on 2025-03-14 09:11_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `red-knot` added by @MichaReiser on 2025-03-14 09:13_

---

_@sharkdp approved on 2025-03-14 11:29_

Looks great, thank you!

---

_Merged by @sharkdp on 2025-03-14 11:29_

---

_Closed by @sharkdp on 2025-03-14 11:29_

---
