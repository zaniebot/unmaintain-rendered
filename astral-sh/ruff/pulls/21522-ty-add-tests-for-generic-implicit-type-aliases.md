```yaml
number: 21522
title: "[ty] Add tests for generic implicit type aliases"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/generic-implicit-aliases-tests
created_at: 2025-11-19T13:11:39Z
updated_at: 2025-11-19T14:06:19Z
url: https://github.com/astral-sh/ruff/pull/21522
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Add tests for generic implicit type aliases

---

_Pull request opened by @sharkdp on 2025-11-19 13:11_

## Summary

Add a set of comprehensive tests for generic implicit type aliases to illustrate the current behavior with many flavors of `@Todo` types and false positive diagnostics.

The tests are partially based on the typing conformance suite, and the expected behavior has been checked against other type checkers.

---

_Label `ty` added by @sharkdp on 2025-11-19 13:11_

---

_Marked ready for review by @sharkdp on 2025-11-19 13:11_

---

_Review requested from @carljm by @sharkdp on 2025-11-19 13:11_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-19 13:11_

---

_Review requested from @dcreager by @sharkdp on 2025-11-19 13:11_

---

_Label `testing` added by @AlexWaygood on 2025-11-19 13:16_

---

_@AlexWaygood approved on 2025-11-19 13:43_

Excellent!

---

_Comment by @AlexWaygood on 2025-11-19 13:45_

I guess one pathological case you could add would be something like

```py
from typing import TypeVar

T = TypeVar("T")
U = TypeVar("U")
S = TypeVar("S")

A = dict[T, U]
B = A[int, S]

def f(x: B[str]):
    reveal_type(x)  # revealed: dict[int, str]
```

---

_Comment by @sharkdp on 2025-11-19 13:57_

> I guess one pathological case

Doesn't seem too pathological. I'll add something like this.

---

_Merged by @sharkdp on 2025-11-19 14:06_

---

_Closed by @sharkdp on 2025-11-19 14:06_

---

_Branch deleted on 2025-11-19 14:06_

---
