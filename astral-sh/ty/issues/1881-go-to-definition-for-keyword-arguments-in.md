```yaml
number: 1881
title: "\"Go to Definition\" for keyword arguments in function call"
type: issue
state: open
author: injust
labels:
  - server
assignees: []
created_at: 2025-12-14T06:47:44Z
updated_at: 2025-12-15T06:30:57Z
url: https://github.com/astral-sh/ty/issues/1881
synced_at: 2026-01-12T15:54:26Z
```

# "Go to Definition" for keyword arguments in function call

---

_@injust_

basedpyright (I tested in Zed) supports "Go to Definition" (cmd-click or F12) for argument names in a function call.

In ty, this is currently a no-op, so you have to use "Go to Definition" on the function, and then find the parameter in its type signature.

Supporting argument names is useful for this pattern, which is used in types-boto3:

```python
class BarTypeDef(TypedDict):
    thing: str


def foo(**kwargs: Unpack[BarTypeDef]) -> None:
    pass


foo(thing="abcd")
#   ^^^^^ cmd-click here
```

In basedpyright, "Go to Definition" on `thing` takes you to its definition in `BarTypeDef`.

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-14 09:22_

---

_Label `server` added by @AlexWaygood on 2025-12-14 10:30_

---

_Comment by @dhruvmanila on 2025-12-15 05:44_

> Supporting kwarg keys is useful for functions with a signature like `def foo(**kwargs: Unpack[BarTypeDef])`, where `BarTypeDef` is a TypedDict. In basedpyright, "Go to Definition" on a kwarg key takes you to the definition of that key in `BarTypeDef`.

Can you clarify what does this mean? Are you saying that when the cursor is at `kwargs` (not the annotated type but the parameter name), basedpyright takes you to `BarTypeDef`?

```py
from typing import TypedDict, Unpack

class BarTypeDef(TypedDict):
    first: int
    second: int


def foo(**kwargs: Unpack[BarTypeDef]): ...
#         ^^^^^^
```

Or, is it that when the cursor is at "first" in `kwargs.first` inside the function body, when you "goto definition", it takes you to the location of the `first` attribute in `BarTypeDef`?

If it's the latter, then for this specific case, it requires https://github.com/astral-sh/ty/issues/1746.

---

_Comment by @injust on 2025-12-15 05:47_

Sorry, it was confusing when I called it a kwarg. I meant keyword/named arguments.

I edited to clarify.

---

_Renamed from ""Go to Definition" for kwarg keys" to ""Go to Definition" for argument name in function call" by @injust on 2025-12-15 05:48_

---

_Renamed from ""Go to Definition" for argument name in function call" to ""Go to Definition" for argument names in function call" by @injust on 2025-12-15 05:53_

---

_Comment by @injust on 2025-12-15 05:58_

My specific example would need https://github.com/astral-sh/ty/issues/1746 first, though.

---

_Comment by @dhruvmanila on 2025-12-15 06:28_

Thanks for clarifying. I think we already support goto definition for argument names considering the following example and trying to invoke goto definition on `b`, `c`:

```py
from typing import TypedDict, Unpack

class BarTypeDef(TypedDict):
    thing: str


def foo(a: int, /, b: int, *, c: int, **kwargs: Unpack[BarTypeDef]) -> None:
    pass


foo(1, b=2, c=3 thing="abcd")
```

So, I think this is more specific to supporting variadic arguments or even more specific to keyword variadic that's typed as `Unpack[<TypedDict>]` because I don't think there's any other remaining type here that would allow keyword arguments.

---

_Renamed from ""Go to Definition" for argument names in function call" to ""Go to Definition" for variadic argument names in function call" by @dhruvmanila on 2025-12-15 06:28_

---

_Renamed from ""Go to Definition" for variadic argument names in function call" to ""Go to Definition" for keyword arguments in function call" by @dhruvmanila on 2025-12-15 06:30_

---
