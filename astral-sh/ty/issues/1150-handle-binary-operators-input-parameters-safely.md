---
number: 1150
title: "handle binary operators input parameters safely and special-case `NotImplementedType` as a return type"
type: issue
state: open
author: KotlinIsland
labels:
  - needs-decision
  - wish
  - typing semantics
assignees: []
created_at: 2025-09-08T06:07:07Z
updated_at: 2026-01-09T02:48:16Z
url: https://github.com/astral-sh/ty/issues/1150
synced_at: 2026-01-10T01:48:23Z
---

# handle binary operators input parameters safely and special-case `NotImplementedType` as a return type

---

_Issue opened by @KotlinIsland on 2025-09-08 06:07_

## summary

operators are unsafe when they return `NotImplemented`:

```py
from types import NotImplementedType


class A:
    def __or__(self, other: object) -> str:
        return NotImplemented

class B:
    def __ror__(self, other: object) -> int:
        return 1

x = A() | B()
x # static: str, runtime: int
```

operators are unsafe when the right hand side is anticipated:

```py
class Left:
    def __add__(self, other: int) -> int:
        # here "other" is Right
        return other + 1  # TypeError here


class Right:
    def __radd__(self, other: Left) -> str:
        return "i'm string :)"

left = Left()
left + 1  # no error, left accepts `int`
left + "" # error, left only accepts `int`
left + Right()  # no error, but left still only accepts `int`
```

# body

https://play.ty.dev/88221963-684d-46c5-a36d-d4c104de716d

here the result is `int | _NotImplementedType`, but it's impossible for it to be `_NotImplementedType` (due to runtime semantics), which should be removed when using the operator syntax

relevant issues:
- https://github.com/DetachHead/basedpyright/issues/1465
- https://github.com/DetachHead/basedpyright/issues/1092

additionally, having `NotImplementedType` in the return position should dicatate the resolution of the result:
https://play.ty.dev/1cfab337-6b92-4c15-9ac5-9bbf42bd1ae9

here, the result should not be `int | NotImplementedType`, it should be `int | str`

we could implement an optional rule that will enforce the usage of `NotImplementedType` into the signature when it is used in a return position, in order to preserve backwards compatibility

### input parameter

for the second case where the the input parameter is passed an invalid type, i would expect an error on the invalid call, and would expect only corectly typed operators to be supported:
```py
class CanRightAdd[T]:
    def __radd__(self, other: T, /) -> object: ...

class Left:
    @overload
    def __add__(self, other: int) -> int: ...
    @overload
    def __add__(self, other: CanRightAdd[Left] & Not[int]) -> NotImplementedType: ...

    def __add__(self, other):
        if isinstance(other, int):
            return other + 1
        return NotImplemented
        

Left() + Right()    # expect no error, and works at runtime 😊
```

---

_Comment by @AlexWaygood on 2025-09-08 09:58_

I'd advise not using `NotImplementedType` in return-type annotations. (I'd probably advise not using it in annotations anywhere, actually!) The convention that typeshed and most other type annotations use is that a dunder method that could return either `T` or the `NotImplemented` singleton is annotated as always returning `T`. To support that convention, ty already implements the opposite special-casing to the one you're asking for here, so that we do not emit an `invalid-return-type` diagnostic on code like this:

```py
class F:
    def __or__(self, other) -> F:
        if not isinstance(other, F):
            return NotImplemented
        return self
```

---

_Comment by @KotlinIsland on 2025-09-08 14:23_

the issue is that it is unsound, the way to correct the ecosystem is to encode the runtime semantics and include `NotImplementedType`, do you have any suggestion as to why this isn't a good idea that would both address the unsoundness described the the op and not break the ecosystem?

---

_Comment by @AlexWaygood on 2025-09-08 14:28_

> the issue is that it is unsound

what specifically are you saying is unsound?

---

_Comment by @KotlinIsland on 2025-09-08 14:59_

the second example in the op

---

_Label `needs-decision` added by @carljm on 2025-09-08 15:12_

---

_Label `typing semantics` added by @carljm on 2025-09-08 15:12_

---

_Comment by @AlexWaygood on 2025-09-08 15:51_

Ah, sorry, I missed the playground link in your OP. Playground links are great, but I find it really helpful if the snippet in question is pasted into an issue body as well -- it makes it easier for me to follow what we're discussing :-)

For other folks' reference, the example is this:

```py
from types import NotImplementedType

class A:
    def __or__(self, other) -> int | NotImplementedType:
        return NotImplemented

class B:
    def __ror__(self, other) -> int:
        return 1

reveal_type(A() | B())  # revealed: int | _NotImplementedType
                        # but at runtime it will always be `int`!
```

But I think I would prefer to solve this by simply issuing a diagnostic if anybody uses `NotImplementedType` in a return-type annotation, where the diagnostic tells the user what to do instead. As you've pointed out, we won't understand it correctly if they do use it in a return-type annotation; and doing so runs counter to the conventional way that methods like this are annotated in the Python ecosystem.

---

_Comment by @carljm on 2025-09-08 15:54_

I think I agree with the OP here that, all else equal, explicitly using `NotImplementedType` in return-type annotations would be a strictly more-capable and more-sound way of annotating these dunder methods, compared to the status-quo approach. And I think we could actually support it without breaking users of the current approach? So I think it's certainly something worth considering. But I don't think it's high priority, certainly not right now, so I'd be inclined to leave this open but defer it.

---

_Comment by @AlexWaygood on 2025-09-08 16:05_

Perhaps it might be strictly more capable and strictly more sound to explicitly use `NotImplementedType` in a return annotation. Supporting both conventions at once in ty might be tricky, though. I assume the desirable state here is something like this?

```py
from typing import overload, Self
from types import NotImplementedType

class F:
    @overload
    def __or__(self, other: "F") -> Self: ...
    @overload
    def __or__(self, other: object) -> NotImplementedType: ...
    def __or__(self, other: object) -> "F" | NotImplementedType:
        if isinstance(other, F):
            return self
        return NotImplemented

F() | 42  # this will raise TypeError at runtime, so we should emit a diagnostic
          # but that wouldn't be detected via our current logic
          # (the `__or__` call "succeeds", it just returns `NotImplemented`)
```

and we'd also need to rework our logic that figures out whether `__or__` or `__ror__` takes precedence, etc. etc.

It would be a fair amount of work for us, and all to support an alternative convention that's far more verbose than the current, widely used convention. I think the benefits here would be really quite marginal.

---

_Comment by @KotlinIsland on 2025-09-08 23:00_

> For other folks' reference, the example is this:

that's the wrong example, the second one demonstrated the unsoundness 

i've updated the op to be clearer

---

_Comment by @AlexWaygood on 2025-09-09 07:18_

Hmm, thanks. That's interesting for sure, but the unsoundness only occurs there because both dunders have their parameters unannotated, right?

---

_Comment by @KotlinIsland on 2025-09-09 07:45_

> Hmm, thanks. That's interesting for sure, but the unsoundness only occurs there because both dunders have their parameters unannotated, right?

no, it's unsound because it's retuning `NotImplemented` which the calls the RHS, and the assumption that this will never happen is unsound


---

_Comment by @AlexWaygood on 2025-09-09 07:58_

Right. But under the existing convention, a dunder method's parameters should be annotated as only accepting types which would _not_ cause that dunder method to return `NotImplemented`. If `other` is annotated with `Never` in `A.__or__`, reflecting the fact that there is no type that can be passed in which will cause the method not to return `NotImplemented`, ty infers the correct result: https://play.ty.dev/88221963-684d-46c5-a36d-d4c104de716d

I suppose we currently don't do any checks that validate this, though. E.g. for this example, we should probably complain that the method is annotated as accepting `object`, but actually just returns `NotImplemented` if `object` is passed in: https://play.ty.dev/88221963-684d-46c5-a36d-d4c104de716d

---

_Comment by @KotlinIsland on 2025-09-09 14:30_

this is completely true, but it's not possible to represent every possible condition in the type system

also, this is especially true for `__eq__`, which **must** accept `object`

---

_Comment by @KotlinIsland on 2025-09-14 10:27_

> a dunder method's parameters should be annotated as only accepting types which would not cause that dunder method to return `NotImplemented`

@AlexWaygood actually, this is not true, see #1181

```py
class Left:
    def __add__(self, other: int) -> int:
        # at runtime, "other" is `Right`
        return other + 1  # TypeError here


class Right:
    def __radd__(self, other: Left) -> str:
        return "i'm string :)"


Left() + Right()
```

---

_Comment by @carljm on 2025-09-15 19:45_

#1181 doesn't demonstrate anything that contradicts what @AlexWaygood said, as far as I can see.

---

_Comment by @KotlinIsland on 2025-09-16 04:40_

> #1181 doesn't demonstrate anything that contradicts what @AlexWaygood said, as far as I can see.

> a dunder method's parameters should be annotated as only accepting types which would not cause that dunder method to return NotImplemented.

i understand your perspective, I feel this statement results in unsafe code, so I disagree with it and consider it not true. i believe "a method should be annotated with the type that it accepts"


---

_Renamed from "special-case `NotImplementedType` as a return type on operator methods" to "handle binary operators input parameters safely and special-case `NotImplementedType` as a return type" by @KotlinIsland on 2025-09-17 04:25_

---

_Comment by @carljm on 2025-10-23 18:09_

There's some "upstream" discussion of this in https://github.com/python/typing/issues/1548

---

_Label `wish` added by @carljm on 2026-01-09 02:48_

---
