```yaml
number: 2173
title: "Some operations on `NewType`s do not preserve their `NewType`-ness"
type: issue
state: closed
author: MeGaGiGaGon
labels: []
assignees: []
created_at: 2025-12-22T22:19:16Z
updated_at: 2025-12-22T22:37:57Z
url: https://github.com/astral-sh/ty/issues/2173
synced_at: 2026-01-12T15:54:26Z
```

# Some operations on `NewType`s do not preserve their `NewType`-ness

---

_@MeGaGiGaGon_

### Summary

Trying to use `NewType`s extensively for the first time, I came across this:
https://play.ty.dev/9d990249-62d6-4ba8-b490-09ab28dd0bf0
```py
from typing import NewType

MyList = NewType("MyList", list[int])

reveal_type(MyList([])[:])  # Revealed type: `list[int]`, should still be MyList
```
Since at runtime a `NewType` just returns the input, slicing a `NewType` of a `list` should still be considered that `NewType`, since it's all just lists at runtime anyways.

One additional thing that confused me when I found this, classes do not behave the same as `NewType`s for this. The slice of a class inheriting from `list` will be `list` unless the method is overwritten, so the current behavior is correct for normal classes.

### Version

ty 0.0.5 (d37b7dbd9 2025-12-20) playground 664686bdb

---

_Renamed from "Slicing `NewType` `list`s does not preserve the `NewType`-ness" to "Some operations on `NewType`s do not preserve their `NewType`-ness" by @MeGaGiGaGon on 2025-12-22 22:30_

---

_Comment by @carljm on 2025-12-22 22:30_

Hmm, this is behavior that all type-checkers agree on. It comes directly from the annotation of `list.__getitem__` in typeshed, which is annotated as returning `list[T]` (not `Self`) when it receives a slice as index. As you point out, this is correct for any normal subclass of list. I'm not convinced that `NewType` of list should be special-cased to behave any differently than a regular subclass. The list returned from slicing at runtime is a new list, it's not the old list. And what would the behavior of slicing a `NewType` of a subclass of `list` be? It cannot be the `NewType`, since at runtime it will be a regular `list`, which would make for an odd inconsistency.

We can't really say what the intended semantics of a `NewType` of `list` are. It could imply some characteristic of the arrangement of list elements (or the length of the list, etc) which could be violated by slicing. (Though obviously not by a no-op slice as in your example -- but that would be an even more oddly specific thing to special-case.)

---

_Closed by @carljm on 2025-12-22 22:30_

---

_Comment by @MeGaGiGaGon on 2025-12-22 22:33_

I see, that makes sense. I didn't consider if the `NewType` had other properties than just being a distinct type.

---

_Comment by @AlexWaygood on 2025-12-22 22:34_

Our behaviour here is also explicitly specified in the [typing-module documentation](https://docs.python.org/3/library/typing.html#newtype):

> You may still perform all `int` operations on a variable of type `UserId`, but the result will always be of type `int`. This lets you pass in a `UserId` wherever an `int` might be expected, but will prevent you from accidentally creating a `UserId` in an invalid way:
> 
> ```py
> # 'output' is of type 'int', not 'UserId'
> output = UserId(23413) + UserId(54341)
> ```

There's similar language in the spec at https://typing.python.org/en/latest/spec/aliases.html#newtype too

---
