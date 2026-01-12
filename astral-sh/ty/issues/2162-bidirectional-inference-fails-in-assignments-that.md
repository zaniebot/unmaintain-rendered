```yaml
number: 2162
title: Bidirectional inference fails in assignments that use unpacking
type: issue
state: open
author: AlexWaygood
labels:
  - bidirectional inference
  - typeddict
assignees: []
created_at: 2025-12-22T15:27:00Z
updated_at: 2025-12-23T06:14:26Z
url: https://github.com/astral-sh/ty/issues/2162
synced_at: 2026-01-12T15:54:26Z
```

# Bidirectional inference fails in assignments that use unpacking

---

_@AlexWaygood_

E.g.

```py
from typing import TypedDict

class Foo(TypedDict):
    x: int

x: Foo

# error: Object of type `dict[Unknown | str, Unknown | int]` is not assignable to `Foo`
x, _ = {"x": 42}, 56
```

Probably not a top priority, but an inconsistency I noticed when looking at https://github.com/astral-sh/ty/issues/2161

---

_Label `bidirectional inference` added by @AlexWaygood on 2025-12-22 15:27_

---

_Added to milestone `Stable` by @carljm on 2025-12-22 16:34_

---

_Comment by @carljm on 2025-12-22 16:36_

Looks like neither Pyright nor Pyrefly support this either. Mypy seems to.

---

_Label `typeddict` added by @dhruvmanila on 2025-12-23 06:14_

---
