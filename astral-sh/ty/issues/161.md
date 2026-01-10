```yaml
number: 161
title: support Protocols
type: issue
state: closed
author: carljm
labels:
  - typing semantics
  - Protocols
assignees: []
created_at: 2025-03-26T12:11:34Z
updated_at: 2025-10-17T09:47:30Z
url: https://github.com/astral-sh/ty/issues/161
synced_at: 2026-01-10T02:06:24Z
```

# support Protocols

---

_Issue opened by @carljm on 2025-03-26 12:11_

Subtasks (these do not all need to be completed before the alpha):

- [x] Understand `Protocol`-instance types as a different kind of type to other `Instance` types (structural types vs nominal types)
- [ ] Implement subtyping and assignability relations between different structural types, and between structural types and nominal types
- [x] Emit errors on invalid `Protocol` class definitions
- [x] Emit errors on `isinstance()` and `issubclass()` runtime checks against `Protocol` classes that are not decorated with `@runtime_checkable`
- [ ] Experiment with removing existing `Type` variants such as `Type::AlwaysTruthy` and replacing them with `Protocol` types

PRs:
- https://github.com/astral-sh/ruff/pull/17436
- https://github.com/astral-sh/ruff/pull/17446
- https://github.com/astral-sh/ruff/pull/17450
- https://github.com/astral-sh/ruff/pull/17452
- https://github.com/astral-sh/ruff/pull/17487
- https://github.com/astral-sh/ruff/pull/17488
- https://github.com/astral-sh/ruff/pull/17525
- https://github.com/astral-sh/ruff/pull/17550
- https://github.com/astral-sh/ruff/pull/17551
- https://github.com/astral-sh/ruff/pull/17556
- https://github.com/astral-sh/ruff/pull/17561
- astral-sh/ruff#17597
- https://github.com/astral-sh/ruff/pull/17603
- https://github.com/astral-sh/ruff/pull/17703
- https://github.com/astral-sh/ruff/pull/17682
- https://github.com/astral-sh/ruff/pull/17741

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-04-04 18:42_

---

_Renamed from "[red-knot] support Protocols" to "support Protocols" by @MichaReiser on 2025-05-07 15:26_

---

_Added to milestone `alpha` by @MichaReiser on 2025-05-07 15:54_

---

_Label `typing semantics` added by @AlexWaygood on 2025-05-11 07:52_

---

_Label `Protocols` added by @AlexWaygood on 2025-05-14 11:03_

---

_Removed from milestone `Alpha` by @MichaReiser on 2025-05-16 12:19_

---

_Added to milestone `Beta` by @MichaReiser on 2025-05-16 12:19_

---

_Comment by @AlexWaygood on 2025-10-17 09:47_

While there are still some large and obvious holes in our `Protocol` implementation, I don't consider that this issue blocks the beta anymore, so I'll close this and open some followup issues for the remaining tasks.

---

_Closed by @AlexWaygood on 2025-10-17 09:47_

---
