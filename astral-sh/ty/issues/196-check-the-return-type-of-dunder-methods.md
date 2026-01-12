```yaml
number: 196
title: Check the return type of dunder methods
type: issue
state: open
author: MichaReiser
labels:
  - runtime semantics
assignees: []
created_at: 2025-02-19T10:13:21Z
updated_at: 2025-05-11T07:25:17Z
url: https://github.com/astral-sh/ty/issues/196
synced_at: 2026-01-12T15:54:22Z
```

# Check the return type of dunder methods

---

_@MichaReiser_

### Description

Dunder methods like `__bool__`, `__len__` (and binary/compare?) should return a value that is assignable to what's expected by their "convention". We currently don't check this. 



---

_Comment by @AlexWaygood on 2025-02-19 11:03_

I'm still not sure that this is actually something a type checker should check, rather than a type-aware linter. The type checker should check (where necessary/appropriate) that the return type is correct when these methods are (explicitly or implicitly) called. But if they're never called, there's no type safety issue. And we already have linter rules to guard against these antipatterns:
- https://docs.astral.sh/ruff/rules/invalid-length-return-type/
- https://docs.astral.sh/ruff/rules/invalid-bool-return-type/
- https://docs.astral.sh/ruff/rules/invalid-index-return-type/
- https://docs.astral.sh/ruff/rules/invalid-str-return-type/
- https://docs.astral.sh/ruff/rules/invalid-bytes-return-type/
- https://docs.astral.sh/ruff/rules/invalid-hash-return-type/

---

_Comment by @MichaReiser on 2025-02-19 11:11_

Yeah sorry. That's what I meant. We should check the return type when implicitly calling those operations and error if the dunder is incorrectly implemented.

---

_Comment by @AlexWaygood on 2025-02-19 11:15_

Okay, great! For `__len__`, we have a TODO here for this that's relevant for this issue:

https://github.com/astral-sh/ruff/blob/e84985e9b3d101d5bd0b4b475855f9e5ccc0a007/crates/red_knot_python_semantic/src/types.rs#L1519

> (and binary/compare?)

In general, these can return arbitrary objects and the call still succeeds, so there's nothing more for us to do on these, I don't think:

```pycon
>>> class Foo:
...     def __lt__(self, other):
...         return Foo()
...         
>>> Foo() < Foo()
<__main__.Foo object at 0x1031cca50>
```

You could argue it's a bit of an antipattern to return a non-bool from these methods, but again, it's sort of a linter issue than something for a type checker to flag

---

_Comment by @carljm on 2025-02-19 19:39_

Yeah I think this would apply to at least `__len__`, `__bool__`, `__str__`, `__bytes__` and `__repr__`. Not sure if there are others I'm forgetting, haven't cross-checked with a comprehensive list of dunders.

---

_Renamed from "[red-knot] Check the return type of dunder methods" to "Check the return type of dunder methods" by @MichaReiser on 2025-05-07 15:26_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-05-11 07:25_

---
