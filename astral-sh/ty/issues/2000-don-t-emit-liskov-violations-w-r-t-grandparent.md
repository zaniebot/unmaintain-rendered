```yaml
number: 2000
title: "Don't emit Liskov violations w.r.t. grandparent class if the violation cannot be fixed without breaking Liskov w.r.t. the parent class"
type: issue
state: closed
author: t-moe
labels:
  - bug
assignees: []
created_at: 2025-12-17T11:50:26Z
updated_at: 2026-01-10T00:27:58Z
url: https://github.com/astral-sh/ty/issues/2000
synced_at: 2026-01-12T15:54:26Z
```

# Don't emit Liskov violations w.r.t. grandparent class if the violation cannot be fixed without breaking Liskov w.r.t. the parent class

---

_@t-moe_

### Question

I have the following code:

```python
from typing import Iterable
from collections import UserList

class Errors(UserList[str]):
    def __init__(self, initlist=None):
        super().__init__(initlist)

    def append(self, item: str) -> None:
        super().append(item)

    def extend(self, other: Iterable[str]) -> None:
        for item in other:
            self.append(item)
```

This fails with an error similar to 

```
 uv run ty check
error[invalid-method-override]: Invalid override of method `append`
    --> error.py:8:9
     |
   6 |         super().__init__(initlist)
   7 |
   8 |     def append(self, item: str) -> None:
     |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Definition is incompatible with `MutableSequence.append`
   9 |         super().append(item)
     |
    ::: stdlib/typing.pyi:1112:9
     |
1110 |     def __delitem__(self, index: slice) -> None: ...
1111 |     # Mixin methods
1112 |     def append(self, value: _T) -> None:
     |         ------------------------------- `MutableSequence.append` defined here
1113 |         """S.append(value) -- append value to the end of the sequence"""
     |
info: This violates the Liskov Substitution Principle
info: rule `invalid-method-override` is enabled by default

error[invalid-method-override]: Invalid override of method `extend`
    --> error.py:11:9
     |
   9 |         super().append(item)
  10 |
  11 |     def extend(self, other: Iterable[str]) -> None:
     |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Definition is incompatible with `MutableSequence.extend`
  12 |         for item in other:
  13 |             self.append(item)
     |
    ::: stdlib/typing.pyi:1118:9
     |
1116 |         """S.clear() -> None -- remove all items from S"""
1117 |
1118 |     def extend(self, values: Iterable[_T]) -> None:
     |         ------------------------------------------ `MutableSequence.extend` defined here
1119 |         """S.extend(iterable) -- extend sequence by appending elements from the iterable"""
     |
info: This violates the Liskov Substitution Principle
info: rule `invalid-method-override` is enabled by default
Found 2 diagnostics
```

What am I missing?
Forgive me, if there is an obvious error hidden in plain sight. I havent written a lot of python lately..

### Version

ty 0.0.2

---

_Label `question` added by @t-moe on 2025-12-17 11:50_

---

_Comment by @AlexWaygood on 2025-12-17 12:08_

Hey @t-moe, thanks for the bug report!

The issue here is with the parameter names. If I have a class hierarchy like this, then instances of `Bar` cannot be used as a drop-in substitute wherever instances of `Foo` are expected, because you can call `Foo.method()` with a `x=` keyword argument, but the same call fails on an instance of `Bar`:

```pycon
>>> class Foo:
...     def method(self, x: int): ...
...     
>>> class Bar(Foo):
...     def method(self, y: int): ...
...     
>>> Foo().method(x=42)
>>> Bar().method(x=42)
Traceback (most recent call last):
  File "<python-input-3>", line 1, in <module>
    Bar().method(x=42)
    ~~~~~~~~~~~~^^^^^^
TypeError: Bar.method() got an unexpected keyword argument 'x'
```

In your code here, your parameter name `item` you've given to `Errors.append` matches the parameter name for the method on `Errors`'s immediate parent class (`UserList`), but doesn't match the parameter name for the method on its _grandparent_ class (`MutableSequence`). So it would be okay to use an instance of `Errors` wherever a `UserList[str]` is accepted, but it _wouldn't_ be okay to use an instance of `Errors` wherever a `MutableSequence[str]` is accepted -- this violates the Liskov Substitution Principle.

Unfortunately... there's not much you can do about it here, in this situation! If you change your parameter names so that they match the parameter names on `MutableSequence.append()`, `Errors` will no longer violate the Liskov Substitution Principle with respect to `MutableSequence`, but it will now violate the Liskov Substitution Principle with respect to `UserList`. So you're in a bit of a bind, because of the fact that `UserList` itself violates the Liskov Substitution Principle.

I think there are several things we need to do here:
1. We need to improve the error message to make clear that it's the parameter names that are the problem! This is tracked in https://github.com/astral-sh/ty/issues/1644
2. Ideally I think we would split errors about bad parameter _names_ into their own, more specific error code. Mypy doesn't emit these errors at all, and pyright doesn't emit them for dunder methods. This, like #1644, is also blocked by https://github.com/astral-sh/ty/issues/163 right now.
3. If we detect that it would actually be impossible to fix the Liskov violation without introducing a new (worse) violation, then we should probably avoid emitting the diagnostic.

---

_Label `question` removed by @AlexWaygood on 2025-12-17 12:08_

---

_Label `bug` added by @AlexWaygood on 2025-12-17 12:08_

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-17 12:08_

---

_Comment by @carljm on 2025-12-18 00:26_

@AlexWaygood is this issue left open in order to track item (3)? Should we re-title it to reflect that? Do we have an issue for (2)?

---

_Renamed from "`invalid-method-override` false positive when subclassing `UserList` ?" to "Don't emit Liskov violations w.r.t. grandparent class if the violation cannot be fixed without breaking Liskov w.r.t. the parent class" by @AlexWaygood on 2025-12-18 14:31_

---

_Comment by @AlexWaygood on 2025-12-18 14:32_

> [@AlexWaygood](https://github.com/AlexWaygood) is this issue left open in order to track item (3)? Should we re-title it to reflect that? Do we have an issue for (2)?

Yes. I have retitled the issue to reflect that, and opened https://github.com/astral-sh/ty/issues/2073 for splitting the more pedantic Liskov checks into separate error codes.

---

_Comment by @The-Red-Dragons on 2025-12-25 13:39_

### Real-world use case: discord.py Modal overrides

I'm experiencing this with discord.py's `Modal` class:

**Inheritance chain:**
- `discord.ui.BaseView.on_error(self, interaction, error, item)` (grandparent)
- `discord.ui.Modal.on_error(self, interaction, error, /)` (parent - already breaks Liskov)
- `MyModal.on_error(self, interaction, error, /)` (child - correctly overrides Modal)

```python
class MyModal(discord.ui.Modal):
    async def on_error(self, interaction: discord.Interaction, error: Exception, /) -> None:
        # ty warns: Invalid override of method 'on_error' - references BaseView.on_error
        # But this correctly matches Modal.on_error signature
        await interaction.response.send_message("Error occurred", ephemeral=True)

---

_Removed from milestone `Stable` by @AlexWaygood on 2025-12-25 13:41_

---

_Added to milestone `Pre-stable 1` by @AlexWaygood on 2025-12-25 13:41_

---

_Comment by @carljm on 2025-12-27 19:09_

https://github.com/astral-sh/ty/issues/2237 observes another real-world case in the standard library (`io.TextIOWrapper.write`)

---

_Comment by @oakif on 2026-01-08 16:57_

+1 on this. we have `WorksheetLine(UserList[Cell])` overriding `__setitem__` and `Worksheet(UserList[WorksheetLine])` overriding `extend`. both with signatures matching `UserList`, but ty reports violations against `MutableSequence`. Working around with `# ty: ignore[invalid-method-override]` for now (ty 0.0.9)

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-09 20:44_

---

_Closed by @charliermarsh on 2026-01-10 00:27_

---
