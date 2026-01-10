```yaml
number: 2088
title: "error on `isinstance` of generic alias, since this fails at runtime"
type: issue
state: closed
author: carljm
labels:
  - runtime semantics
assignees: []
created_at: 2025-12-18T20:25:11Z
updated_at: 2025-12-18T21:08:52Z
url: https://github.com/astral-sh/ty/issues/2088
synced_at: 2026-01-10T01:54:00Z
```

# error on `isinstance` of generic alias, since this fails at runtime

---

_Issue opened by @carljm on 2025-12-18 20:25_

Right now we are fine with `isinstance(x, list[str])` (and we even do "the right thing" from a type system perspective), but this check actually fails at runtime, we should error on it.

---

_Added to milestone `Stable` by @carljm on 2025-12-18 20:25_

---

_Label `runtime semantics` added by @carljm on 2025-12-18 20:25_

---

_Comment by @AlexWaygood on 2025-12-18 20:26_

This is a duplicate of https://github.com/astral-sh/ty/issues/116 :-)

---

_Comment by @carljm on 2025-12-18 20:33_

Argh, I searched but that issue doesn't specifically discuss `isinstance`. You're right that fixing that is how we should fix this, though.

---

_Closed by @carljm on 2025-12-18 20:33_

---

_Comment by @AlexWaygood on 2025-12-18 20:35_

> Argh, I searched but that issue doesn't specifically discuss `isinstance`.

It does if you expand the "REPL session demonstrating several places where generic aliases are not accepted as instances of `type`" section ;-)

---

_Comment by @carljm on 2025-12-18 21:08_

Ah well then it's just GH issue search being useless as usual 😆

---
