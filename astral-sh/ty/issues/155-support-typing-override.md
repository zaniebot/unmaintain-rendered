```yaml
number: 155
title: "support `@typing.override`"
type: issue
state: open
author: carljm
labels:
  - typing semantics
assignees: []
created_at: 2025-03-26T18:06:17Z
updated_at: 2025-11-26T19:29:40Z
url: https://github.com/astral-sh/ty/issues/155
synced_at: 2026-01-10T01:58:59Z
```

# support `@typing.override`

---

_Issue opened by @carljm on 2025-03-26 18:06_

- [x] (basic) error if a method decorated `@override` does not override anything
- [ ] (strict) error if a method that overrides a super method is not decorated with `@override`

---

_Renamed from "[red-knot] support @typing.override" to "support @typing.override" by @MichaReiser on 2025-05-07 15:25_

---

_Label `typing semantics` added by @AlexWaygood on 2025-05-11 07:55_

---

_Removed from milestone `GA` by @carljm on 2025-06-11 00:50_

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:50_

---

_Removed from milestone `Beta` by @carljm on 2025-08-15 15:00_

---

_Added to milestone `GA` by @carljm on 2025-08-15 15:00_

---

_Renamed from "support @typing.override" to "support `@typing.override`" by @AlexWaygood on 2025-10-06 16:38_

---

_Comment by @AlexWaygood on 2025-11-26 19:27_

Following https://github.com/astral-sh/ruff/pull/21627, we now support basic enforcement of `@override`, but not yet the strict check described in https://typing.python.org/en/latest/spec/class-compat.html#strict-enforcement-per-project.

---
