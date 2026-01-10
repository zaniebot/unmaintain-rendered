```yaml
number: 16305
title: "[red-knot] Add support for `@classmethod`s"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/classmethods
created_at: 2025-02-21T13:16:20Z
updated_at: 2025-02-24T08:55:38Z
url: https://github.com/astral-sh/ruff/pull/16305
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Add support for `@classmethod`s

---

_Pull request opened by @sharkdp on 2025-02-21 13:16_

## Summary

Add support for `@classmethod`s.

```py
class C:
    @classmethod
    def f(cls, x: int) -> str:
        return "a"

reveal_type(C.f(1))  # revealed: str
```

## Test Plan

New Markdown tests

---

_Label `red-knot` added by @sharkdp on 2025-02-21 13:16_

---

_Review requested from @carljm by @sharkdp on 2025-02-21 13:16_

---

_Review requested from @MichaReiser by @sharkdp on 2025-02-21 13:16_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-02-21 13:16_

---

_Renamed from "David/classmethods" to "[red-knot] Add support for `@classmethod`s" by @sharkdp on 2025-02-21 13:16_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:3251 on 2025-02-21 13:17_

It happened again. This was missing previously.

---

_@sharkdp reviewed on 2025-02-21 13:17_

---

_@sharkdp reviewed on 2025-02-21 13:24_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/methods.md`:274 on 2025-02-21 13:24_

This could be `Literal[C]` instead of `type[C]`, but everything works anyway. This happens because we turn `Type::Instance("C")` into its meta type when a classmethod is called on an instance of `C`. This corresponds to the `type(obj)` call in this (pseudo) implementation:
```py
class classmethod:
    # …
    def __get__(self, obj, cls=None):
        if cls is None:
            cls = type(obj)
        return MethodType(self.f, cls)
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:3978 on 2025-02-21 13:37_

Would it make sense to have a bitflag on `FunctionType` where we initialize well known decorators?

---

_@MichaReiser reviewed on 2025-02-21 13:37_

---

_Review comment by @Skylion007 on `crates/red_knot_python_semantic/src/types.rs`:1952 on 2025-02-21 14:27_

Isn't restricting the number of decorators to 1 overly restrictive? Like one wont be able to wrap them with relatively harmless decorators like deprecated or a noop decorator etc without breaking typing?

---

_@Skylion007 reviewed on 2025-02-21 14:27_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1952 on 2025-02-21 14:30_

> Isn't restricting the number of decorators to 1 overly restrictive?

Yes — of course. I put that in place because we do not understand decorators yet. So if a `@classmethod` is additionally decorated with something else, we don't support that yet (and will infer `@Todo(…)` types). I should probably add a TODO comment here to make the intention behind this check more apparent — thanks.

> Like one wont be able to wrap them with relatively harmless decorators like deprecated or a noop decorator etc without breaking typing?

We certainly hope to support all of this eventually.

---

_@sharkdp reviewed on 2025-02-21 14:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/methods.md`:332 on 2025-02-21 14:31_

we will want to infer a more precise type at some point, I think. `classmethod` and `staticmethod` have additional attributes on Python 3.10+ that other descriptors do not have: https://github.com/python/typeshed/blob/ac8f2632ec37bb4a82ade0906e6ce9bdb33883d3/stdlib/builtins.pyi#L137-L168

But this is fine for now!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3251 on 2025-02-21 15:22_

Time we took action on this comment, I think...

https://github.com/astral-sh/ruff/blob/b9b094869ac6af590c910fde48b4fd3e01601236/crates/red_knot_python_semantic/src/types.rs#L2990-L2992

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/signatures.rs`:33 on 2025-02-21 15:24_

I think this is going to lead to compiler warnings on release builds about unused arguments; we might need to do something similar to the `allow()` here:

https://github.com/astral-sh/ruff/blob/3aa7ba31b135bfeeb73e009415e0debb4ca47b95/crates/red_knot_python_semantic/src/symbol.rs#L55-L58

---

_@AlexWaygood reviewed on 2025-02-21 15:24_

---

_@MichaReiser reviewed on 2025-02-21 15:26_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:3251 on 2025-02-21 15:26_

We could derive iter for the enum and add a unit tests over a macro. 

---

_@AlexWaygood reviewed on 2025-02-21 16:00_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3251 on 2025-02-21 16:00_

I think deriving iter requires adding a red-knot dependency on `strum` and `strum_macros`? Not that I object to that if we think it's worth it

---

_@sharkdp reviewed on 2025-02-21 18:44_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/methods.md`:332 on 2025-02-21 18:44_

> we will want to infer a more precise type at some point, I think. `classmethod` and `staticmethod` have additional attributes on Python 3.10+ that other descriptors do not have

Yes, that's why I noted it down. A previous version included (partial?) support for this, but it would require us to add one more special-case callable type, so I removed that again, as it didn't seem that important  (f273460696fbacb8b0f279b7fe52def97d5946ea), and because I think it might change again once we have more general callable types and support for decorators.

---

_@MichaReiser reviewed on 2025-02-21 18:47_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:3251 on 2025-02-21 18:47_

A "cheap" version of this could be to add a comment before the first and after the last variant in `KnownClass`, both telling you that you absolutely have to make sure to add the variant to `try_from_file_and_name`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/methods.md`:274 on 2025-02-21 21:06_

I think it might be useful to test accessing and calling a classmethod from a subclass that doesn't override it? This would ensure that we aren't placing an overly restrictive type on the `cls` arg.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/methods.md`:297 on 2025-02-21 21:09_

I would expect this `D` name reference to fail to resolve, but somehow it doesn't? This isn't a stub file, and doesn't have `from __future__ import annotations`, so name references in annotations should resolve eagerly, and the name `D` should not be bound here yet.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3978 on 2025-02-21 21:21_

Maybe. I think this should wait until we have proper decorator support, at which point we won't track all decorators on `FunctionType` anymore at all. It may be that we still want to have a "shortcut" where we represent `classmethod(some_function)` as just a `FunctionType` with a flag on it, and similar for `staticmethod`. But that seems like the right time to optimize this representation, since all of this is temporary.

---

_@carljm approved on 2025-02-21 21:29_

Looks great!

---

_@sharkdp reviewed on 2025-02-22 19:35_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/methods.md`:297 on 2025-02-22 19:35_

> I would expect this `D` name reference to fail to resolve, but somehow it doesn't?

Ah, I was wondering about this, because one of my other examples failed when I ran it through CPython. I then added `from __future__ import annotations`, but forgot to write it down as a (pre existing?) issue. I'll do so.

---

_@AlexWaygood reviewed on 2025-02-23 21:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3251 on 2025-02-23 21:50_

#16326 for a possible solution!

---

_@sharkdp reviewed on 2025-02-24 08:37_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/methods.md`:297 on 2025-02-24 08:37_

This particular test already includes `from __future__ import annotations` at the top.

Reported as https://github.com/astral-sh/ruff/issues/16341

---

_@sharkdp reviewed on 2025-02-24 08:47_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1952 on 2025-02-24 08:47_

Added a new test that shows that we do not support classmethods that are also wrapped with other decorators.

---

_Merged by @sharkdp on 2025-02-24 08:55_

---

_Closed by @sharkdp on 2025-02-24 08:55_

---

_Branch deleted on 2025-02-24 08:55_

---
