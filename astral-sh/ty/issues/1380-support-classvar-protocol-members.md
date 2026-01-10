```yaml
number: 1380
title: "Support `ClassVar` protocol members"
type: issue
state: open
author: AlexWaygood
labels:
  - Protocols
assignees: []
created_at: 2025-10-17T09:56:25Z
updated_at: 2025-10-17T09:56:58Z
url: https://github.com/astral-sh/ty/issues/1380
synced_at: 2026-01-10T02:06:25Z
```

# Support `ClassVar` protocol members

---

_Issue opened by @AlexWaygood on 2025-10-17 09:56_

Consider the following two protocols:

```py
from typing import Protocol, ClassVar

class P1(Protocol):
    x: int

class P2(Protocol):
    x: ClassVar[int]
```

In order for a type `N` to satisy `P1`'s interface, an `x` attribute needs to be readable with type `int` on instances of `N` and also writable with type `int` on instances of `N`. The `x` attribute does not need to be readable or writable on instances of `type[N]`.

In order for a type `N` to satsify `P2`'s interface, however, an `s` attribute needs to be readable with type `int` on instances of `N` _and_ instances of `type[N]`. It does not need to be writable on instances of `N`, but it _does_ need to be writable on instances of `type[N]`.

---

_Label `Protocols` added by @AlexWaygood on 2025-10-17 09:56_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-10-17 09:56_

---

_Added to milestone `GA` by @AlexWaygood on 2025-10-17 09:56_

---
