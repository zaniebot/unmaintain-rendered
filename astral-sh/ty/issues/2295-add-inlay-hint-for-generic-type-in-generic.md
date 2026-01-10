---
number: 2295
title: Add inlay hint for generic type in generic constructor calls
type: issue
state: open
author: MeGaGiGaGon
labels:
  - server
  - wish
assignees: []
created_at: 2025-12-31T22:11:49Z
updated_at: 2026-01-06T00:26:10Z
url: https://github.com/astral-sh/ty/issues/2295
synced_at: 2026-01-10T01:51:14Z
---

# Add inlay hint for generic type in generic constructor calls

---

_Issue opened by @MeGaGiGaGon on 2025-12-31 22:11_

### Summary

When writing highly generic-centric code, one of my favorite features from basedpyright that I'd like to see in ty is inlay hints for constructors, specifically for situations where there is no other good place for showing the type of that generic, ie in the middle of a chain of calls.

<img width="430" height="180" alt="Image" src="https://github.com/user-attachments/assets/9bc2fb49-08e2-4bf2-9fa1-5f65d3f1803d" />

Currently ty doesn't have that `[int]` inlay hint on the `HasGeneric` constructor call
https://play.ty.dev/d2ef7feb-51a9-47ba-b349-8c0c15196854

<img width="340" height="181" alt="Image" src="https://github.com/user-attachments/assets/e5b408fa-0032-4d9e-8a84-cbd60dc02f7b" />

Code for easy copy/pasting:
```py
class HasGeneric[T]:
    def __init__(self, x: T):
        self._x: T = x

def uses_generic[T](item: HasGeneric[T]): ...

my_item = 1

uses_generic(HasGeneric(my_item))
```

(At least personally I would also really like it if there was an additional `[int]` inlay hint on the `uses_generic` call, but that is probably too far since it's currently invalid syntax.)

### Version

playground 758926eec

---

_Label `server` added by @AlexWaygood on 2025-12-31 22:34_

---

_Comment by @Gankra on 2026-01-05 12:25_

Ooh that is neat. In simple cases I guess this is as ""easy"" as "get the inferred type of the expression" and then *vague gestures* extracting the generics from the type... but I have a feeling there is 1000 corner cases and this will be quite tricky.

---

_Label `wish` added by @Gankra on 2026-01-05 12:25_

---

_Comment by @carljm on 2026-01-06 00:26_

Yeah that is very cool. I'm optimistic there won't be _that_ many corner cases, and the "vague gestures" should also not be that complicated -- it's in principle exactly the same thing that type-display for a `GenericAlias` already does.

---
