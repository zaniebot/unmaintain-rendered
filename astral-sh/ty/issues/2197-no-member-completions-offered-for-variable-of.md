---
number: 2197
title: "No member completions offered for variable of type `T | Unknown`"
type: issue
state: closed
author: rodrigoscc
labels:
  - bug
  - server
  - completions
assignees: []
created_at: 2025-12-24T03:26:57Z
updated_at: 2026-01-07T15:15:38Z
url: https://github.com/astral-sh/ty/issues/2197
synced_at: 2026-01-10T01:51:14Z
---

# No member completions offered for variable of type `T | Unknown`

---

_Issue opened by @rodrigoscc on 2025-12-24 03:26_

### Summary

Hello,

I've found a case where autocomplete won't work even tho the type of the variable is clear. It is when I iterate over a list, the variable attributes are not autocompleted inside the for loop:
<img width="638" height="347" alt="Image" src="https://github.com/user-attachments/assets/77452a4a-b079-429e-8a2f-e37e612eed87" />

`i` type is correctly detected as `Unknown | A` and I can even go to definition of its attributes, but autocomplete doesn't work. Here's the playground link: https://play.ty.dev/80851291-8503-4aaf-b5aa-f286949c820b

Thanks for this amazing LSP! :)

### Version

0.0.6

---

_Label `bug` added by @AlexWaygood on 2025-12-24 09:32_

---

_Label `server` added by @AlexWaygood on 2025-12-24 09:32_

---

_Label `completions` added by @AlexWaygood on 2025-12-24 09:32_

---

_Comment by @sinon on 2025-12-24 10:16_

The loop aspect doesn't seem to be the cause it's any union of types (not specifically to `Unknown`) seems to be stop the completions.

https://play.ty.dev/937e266f-1665-4b5d-a2c0-d7b4174c3a0d
```py
import random
from ty_extensions import Unknown
class A:
    def __init__(self, msg: str):
        self.msg = msg

class B:
    def __init__(self, msg: str):
        self.message = msg


a = [A("hello"), A("no")]. # list[A | Unknown]
b: list[A] = [A("hello"), A("no")]
c: list[A | B] = [A("hello"), A("no"), B("yes")]

def something() -> A | B:
    return random.choice([A("a"), B("b")])

d = something()

# no completion
d.m

for i in a:
    # no completion
    i.m

for i in b:
    # works
    i.msg

for i in c:
    # no completion
    i.m
```

---

_Renamed from "completion not working for loop variable attributes" to "No member completions offered for variable of type `T | Unknown`" by @AlexWaygood on 2025-12-24 12:50_

---

_Comment by @AlexWaygood on 2025-12-24 12:57_

Thanks @rodrigoscc for the report and thanks @sinon for the initial triage!!

The problem here is [this branch](https://github.com/astral-sh/ruff/blob/81f34fbc8e404cd26fa40ee86a339947dce7617c/crates/ty_python_semantic/src/types/list_members.rs#L165-L172) in our routine for collecting "all members" of a given type. On a union type `T | S`, the available members will be the _intersection_ of `<set of members available on T>` with `<set of members available on S>`. That makes sense from a type-theory perspective, but it's obviously suboptimal for this specific case.

The problem is that a dynamic type such as `Any` or `Unknown` has an infinite number of members available on it, but it would take a "very long time"™️ for us to enumerate an infinite number of members, so instead of attempting to do that the `all_members()` routine just returns an empty set if you ask for all members available on a dynamic type. And the intersection of any set with an empty set is another empty set.

The long and short of it is that this routine is too naive currently when it comes to unions that include dynamic types. Shouldn't be too hard to fix.

---

_Comment by @Wizzerinus on 2025-12-24 13:44_

Honestly I think this approach could be changed in general, not only for unions of dynamic types. Consider, for example, the type `T | None` (with T being some concrete type). While getting members from this type is not sound at runtime, it is still often useful to get autocompletion for its members, for example, i.e. to see what is possible to do with values of type T, without having to insert an `assert obj is not None` before that.

---

_Comment by @AlexWaygood on 2025-12-24 13:47_

> Honestly I think this approach could be changed in general, not only for unions of dynamic types. Consider, for example, the type `T | None` (with T being some concrete type). While getting members from this type is not sound at runtime, it is still often useful to get autocompletion for its members, for example, i.e. to see what is possible to do with values of type T, without having to insert an `assert obj is not None` before that.

Maybe. But wouldn't it be confusing for a user to have a situation where ty suggests an autocompletion, you accept that autocompletion suggestion, and then ty immediately complains about the code that it just added for you?

We should look at what other language servers like pylance do here.

---

_Comment by @Wizzerinus on 2025-12-24 14:01_

Basedpyright gives all (or at least, multiple of) union fields in the completions:

<img width="686" height="542" alt="Image" src="https://github.com/user-attachments/assets/1cb99c32-bbed-43a9-bf90-b451628cb09f" />

<img width="678" height="605" alt="Image" src="https://github.com/user-attachments/assets/20f15893-c183-4305-8c09-b544b8317e41" />

So does pyrefly:

<img width="680" height="651" alt="Image" src="https://github.com/user-attachments/assets/94a0c388-c864-40e9-adea-2f5e68f17640" />

(EDIT: fixed Basedpyright and Pyrefly ss's being swapped)

---

_Comment by @AlexWaygood on 2025-12-24 14:04_

Thanks @Wizzerinus, that's super helpful!

I would still be inclined to view that as a separate issue that should maybe be fixed in a separate PR. But it's extremely valuable to know how other LSPs handle union types here!

---

_Added to milestone `Pre-stable 1` by @AlexWaygood on 2026-01-06 23:54_

---

_Comment by @AlexWaygood on 2026-01-06 23:57_

@zsol mentioned that this was one of his top annoyances with ty right now, and it was also mentioned as a point of frustration in https://github.com/astral-sh/ty/issues/2373, so we should treat this as high-priority

---

_Comment by @carljm on 2026-01-07 00:03_

Though I think if we do #1240 (and make it default) that will reduce the priority on this quite a lot. (We should still do it, of course.)

---

_Closed by @BurntSushi on 2026-01-07 15:15_

---
