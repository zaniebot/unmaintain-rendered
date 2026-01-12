```yaml
number: 17157
title: "[red-knot] Detect division-by-zero in unions and intersections"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/division-by-zero-unions
created_at: 2025-04-02T16:12:36Z
updated_at: 2025-04-02T16:21:29Z
url: https://github.com/astral-sh/ruff/pull/17157
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Detect division-by-zero in unions and intersections

---

_@sharkdp_

## Summary

Yet another instance of "we should have handled unions and intersections correctly". With this PR, we emit a diagnostic for this case where previously didn't:
```py
from typing import Literal

def f(m: int, n: Literal[-1, 0, 1]):
    # error: [division-by-zero] "Cannot divide object of type `int` by zero"
    return m / n
```

The new Boolean flag is not pretty, but without it, we emit multiple diagnostics for a single expression in some cases. Let me know if you have better ways to address this.

## Test Plan

New Markdown test

---

_Label `red-knot` added by @sharkdp on 2025-04-02 16:12_

---

_Review requested from @carljm by @sharkdp on 2025-04-02 16:12_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-02 16:12_

---

_Review requested from @dcreager by @sharkdp on 2025-04-02 16:12_

---

_Comment by @github-actions[bot] on 2025-04-02 16:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@AlexWaygood approved on 2025-04-02 16:19_

---

_@carljm approved on 2025-04-02 16:21_

---

_Merged by @sharkdp on 2025-04-02 16:21_

---

_Closed by @sharkdp on 2025-04-02 16:21_

---

_Branch deleted on 2025-04-02 16:21_

---
