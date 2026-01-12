```yaml
number: 544
title: support typing.TypeAlias (PEP 613)
type: issue
state: closed
author: carljm
labels:
  - typing semantics
assignees: []
created_at: 2025-05-30T00:52:36Z
updated_at: 2025-11-21T02:02:20Z
url: https://github.com/astral-sh/ty/issues/544
synced_at: 2026-01-12T15:54:23Z
```

# support typing.TypeAlias (PEP 613)

---

_@carljm_

If a name is annotated as `typing.TypeAlias`, the RHS should be evaluated as a type expression.

---

_Label `typing semantics` added by @carljm on 2025-05-30 00:52_

---

_Comment by @sharkdp on 2025-06-03 07:12_

A minor note here: Not that this needs to be a blocker, but if we implement this before fixing the recursive types problem, we'll have to move ~10 projects to "bad.txt" ([ref](https://github.com/astral-sh/ruff/pull/18214)), and we probably have to expect more panics on user projects as well. 

---

_Comment by @AlexWaygood on 2025-06-03 07:28_

> A minor note here: Not that this needs to be a blocker, but if we implement this before fixing the recursive types problem, we'll have to move ~10 projects to "bad.txt" ([ref](https://github.com/astral-sh/ruff/pull/18214)), and we probably have to expect more panics on user projects as well.

I'm sort-of surprised that's "all"! Typeshed uses recursive type aliases for some very fundamental functions in the stdlib: https://github.com/astral-sh/ruff/blob/e2d96df50192803abe63cb0865b447c2552c2eff/crates/ty_vendored/vendor/typeshed/stdlib/builtins.pyi#L1533. I'm somewhat uneasy about implementing support for `TypeAlias` unless we're confident that we don't (and won't) panic on typeshed's annotations there.

(Even if we don't _currently_ panic on that alias after support for `TypeAlias` has been added, is there a risk that there's another missing feature that means we don't understand that alias fully right now which, when implemented, suddenly causes us to start panicking?)

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:31_

---

_Assigned to @carljm by @carljm on 2025-08-15 15:07_

---

_Renamed from "support typing.TypeAlias" to "support typing.TypeAlias (PEP 613)" by @AlexWaygood on 2025-10-06 15:51_

---

_Assigned to @ibraheemdev by @ibraheemdev on 2025-10-17 14:41_

---

_Unassigned @carljm by @MichaReiser on 2025-10-17 14:44_

---

_Assigned to @carljm by @carljm on 2025-10-30 15:08_

---

_Unassigned @ibraheemdev by @carljm on 2025-10-30 15:08_

---

_Comment by @carljm on 2025-11-12 02:16_

https://github.com/astral-sh/ruff/pull/21394 will mostly fix this, with the exception of recursive aliases (they don't panic, but the point of recursion becomes `Unknown`) ~~and stringified types on the RHS of a TypeAlias~~. Will fix those separately (the latter pre-beta, the former probably not.)

---

_Closed by @carljm on 2025-11-21 02:00_

---

_Comment by @carljm on 2025-11-21 02:02_

Closing this issue. Full recursive type alias support is tracked in #1604 

---
