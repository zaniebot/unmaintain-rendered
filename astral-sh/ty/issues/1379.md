```yaml
number: 1379
title: "Properly support `@property` members on protocols"
type: issue
state: open
author: AlexWaygood
labels:
  - Protocols
assignees: []
created_at: 2025-10-17T09:52:33Z
updated_at: 2025-10-17T09:52:33Z
url: https://github.com/astral-sh/ty/issues/1379
synced_at: 2026-01-10T02:06:25Z
```

# Properly support `@property` members on protocols

---

_Issue opened by @AlexWaygood on 2025-10-17 09:52_

If a protocol `P` has an `@property` member `foo`, we currently say that any nominal type `N` will satisfy `P`'s interface if `N` has an attribute `foo`. This is obviously incorrect; we should also consider the type of the member `foo` on the protocol. For `@property` members with setters, we should also check whether the `foo` attribute can be set on `N` with the type the setter specifies.

Detailed tests for this are already in place at https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/protocols.md

---

_Added to milestone `GA` by @AlexWaygood on 2025-10-17 09:52_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-10-17 09:52_

---

_Label `Protocols` added by @AlexWaygood on 2025-10-17 09:52_

---
