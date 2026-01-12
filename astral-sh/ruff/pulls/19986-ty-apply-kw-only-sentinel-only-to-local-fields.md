```yaml
number: 19986
title: "[ty] apply `KW_ONLY` sentinel only to local fields"
type: pull_request
state: merged
author: PrettyWood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: fix/1047
created_at: 2025-08-19T09:52:10Z
updated_at: 2025-08-19T19:24:50Z
url: https://github.com/astral-sh/ruff/pull/19986
synced_at: 2026-01-12T15:56:51Z
```

# [ty] apply `KW_ONLY` sentinel only to local fields

---

_@PrettyWood_

fix https://github.com/astral-sh/ty/issues/1047

## Summary

This PR fixes how `KW_ONLY` is applied in dataclasses. Previously, the sentinel leaked into subclasses and incorrectly marked their fields as keyword-only; now it only affects fields declared in the same class.

```py
from dataclasses import dataclass, KW_ONLY

@dataclass
class D:
    x: int
    _: KW_ONLY
    y: str

@dataclass
class E(D):
    z: bytes

# This should work: x=1 (positional), z=b"foo" (positional), y="foo" (keyword-only)
E(1, b"foo", y="foo")

reveal_type(E.__init__)  # revealed: (self: E, x: int, z: bytes, *, y: str) -> None
```

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
mdtests

---

_Review requested from @carljm by @PrettyWood on 2025-08-19 09:52_

---

_Review requested from @AlexWaygood by @PrettyWood on 2025-08-19 09:52_

---

_Review requested from @sharkdp by @PrettyWood on 2025-08-19 09:52_

---

_Review requested from @dcreager by @PrettyWood on 2025-08-19 09:52_

---

_Label `bug` added by @AlexWaygood on 2025-08-19 09:52_

---

_Label `ty` added by @AlexWaygood on 2025-08-19 09:52_

---

_Comment by @github-actions[bot] on 2025-08-19 09:54_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-19 09:56_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@carljm approved on 2025-08-19 18:01_

This is excellent, thank you!

---

_Merged by @carljm on 2025-08-19 18:01_

---

_Closed by @carljm on 2025-08-19 18:01_

---

_Branch deleted on 2025-08-19 19:24_

---
