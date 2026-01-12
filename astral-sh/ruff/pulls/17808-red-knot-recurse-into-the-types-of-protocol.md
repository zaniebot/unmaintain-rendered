```yaml
number: 17808
title: "[red-knot] Recurse into the types of protocol members when normalizing a protocol's interface"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/protocol-union-normalization
created_at: 2025-05-03T10:47:26Z
updated_at: 2025-05-03T15:43:38Z
url: https://github.com/astral-sh/ruff/pull/17808
synced_at: 2026-01-12T15:56:06Z
```

# [red-knot] Recurse into the types of protocol members when normalizing a protocol's interface

---

_@AlexWaygood_

## Summary

Currently red-knot does not understand `Foo` and `Bar` here as being equivalent:

```py
from typing import Protocol

class A: ...
class B: ...
class C: ...

class Foo(Protocol):
    x: A | B | C

class Bar(Protocol):
    x: B | A | C
```

Nor does it understand `A | B | Foo` as being equivalent to `Bar | B | A`. This PR fixes that.

## Test Plan

new mdtest assertions added that fail on `main`


---

_Label `red-knot` added by @AlexWaygood on 2025-05-03 10:47_

---

_Review requested from @carljm by @AlexWaygood on 2025-05-03 10:47_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-03 10:47_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-03 10:47_

---

_Comment by @github-actions[bot] on 2025-05-03 10:52_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-05-03 15:40_

Nice!

---

_Merged by @AlexWaygood on 2025-05-03 15:43_

---

_Closed by @AlexWaygood on 2025-05-03 15:43_

---

_Branch deleted on 2025-05-03 15:43_

---
