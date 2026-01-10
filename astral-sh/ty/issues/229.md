```yaml
number: 229
title: Inconsistency between unbound and undeclared symbols
type: issue
state: open
author: sharkdp
labels:
  - control flow
assignees: []
created_at: 2024-11-12T12:57:19Z
updated_at: 2025-11-18T16:10:24Z
url: https://github.com/astral-sh/ty/issues/229
synced_at: 2026-01-10T01:58:59Z
```

# Inconsistency between unbound and undeclared symbols

---

_Issue opened by @sharkdp on 2024-11-12 12:57_

Pretty much what @carljm wrote in a comment [here](https://github.com/astral-sh/ruff/pull/13980#discussion_r1824747585), and what is now described in this TODO comment:

https://github.com/astral-sh/ruff/blob/13a1483f1e51087d7c95922a6b3d2914ac2b68c3/crates/red_knot_python_semantic/src/types.rs#L80-L93

I'm opening this as a ticket as there seems to be an open discussion point regarding the desired behavior:

> Possibly the behavior here should differ depending on whether we are importing from a stub or a regular Python module? But an argument could be made that even in a regular Python module, if it declares x: int at module level, we should just trust that and not require that we can see the binding of it.

---

_Comment by @mishamsk on 2025-02-28 21:12_

subscribed ;-)

---

_Renamed from "[red-knot] Inconsistency between unbound and undeclared symbols" to "Inconsistency between unbound and undeclared symbols" by @MichaReiser on 2025-05-07 15:27_

---

_Label `control flow` added by @AlexWaygood on 2025-05-10 21:39_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-13 16:00_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
