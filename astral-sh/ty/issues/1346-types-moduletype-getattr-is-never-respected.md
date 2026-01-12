```yaml
number: 1346
title: "`types.ModuleType.__getattr__` is never respected"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - attribute access
assignees: []
created_at: 2025-10-13T13:32:46Z
updated_at: 2025-11-14T12:59:16Z
url: https://github.com/astral-sh/ty/issues/1346
synced_at: 2026-01-12T15:54:25Z
```

# `types.ModuleType.__getattr__` is never respected

---

_@AlexWaygood_

### Summary

Typeshed includes a `__getattr__` method on its stub for `types.ModuleType`. This makes [code like this](https://github.com/AlexWaygood/typeshed-stats/blob/b60f4ff38e5a5cba5ee2a5f14f0f735ff3df58ef/tests/test___all__.py#L33-L35) easier to write: although it is true that not all modules have an `__all__` attribute, I do happen to know in this case that all the submodules of `typeshed_stats` _do_ have `__all__` attributes, and it would be somewhat annoying to have to `type: ignore` these lines. There's no other way of annotating this code to make the type checker happy, and it's pretty often the case that when you're handling a list of `ModuleType` instances in this way in Python, you know where they came from. A little bit of dynamic typing here goes a long way to improve ergonomics.

Ty ignores `ModuleType.__getattr__` when resolving attribute accesses on module _literal_ types, which is good, because otherwise it would consider every module to have every attribute. But it also ignores `ModuleType.__getattr__` when resolving attribute accesses on non-literal instances of `ModuleType`, which doesn't seem right. Ty therefore emits an `unresolved-attribute` diagnostic on this snippet even though mypy, pyright and pyrefly all have no complaints about it:

```py
import types

def f(x: types.ModuleType):
    print(x.__all__)
```

I spotted this by studying the ecosystem report in https://github.com/astral-sh/ruff/pull/20723#issuecomment-3372209543

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2025-10-13 13:32_

---

_Label `attribute access` added by @AlexWaygood on 2025-10-13 13:32_

---

_Comment by @sharkdp on 2025-10-13 14:06_

> This makes [code like this](https://github.com/AlexWaygood/typeshed-stats/blob/b60f4ff38e5a5cba5ee2a5f14f0f735ff3df58ef/tests/test___all__.py#L33-L35) easier to write: although it is true that not all modules have an `__all__` attribute, I do happen to know in this case that all the submodules of `typeshed_stats` _do_ have `__all__` attributes, and it would be somewhat annoying to have to `type: ignore` these lines. There's no other way of annotating this code to make the type checker happy

I'm not generally questioning the usefulness of this typeshed lie, but this specific case doesn't seem that compelling to me? Couldn't you use `hasattr` narrowing?

```py
def test_submodule__all___is_valid(submodule: types.ModuleType) -> None:
    assert hasattr(submodule, "__all__")
    assert isinstance(submodule.__all__, list)
    assert all(isinstance(item, str) for item in submodule.__all__)
```
https://play.ty.dev/c16a6272-d2c7-496a-b9fa-a6513af5ece8



---

_Comment by @AlexWaygood on 2025-10-13 14:13_

> this specific case doesn't seem that compelling to me? Couldn't you use `hasattr` narrowing?

True, though I run pyright in CI as well as mypy, and IIRC pyright does not support `hasattr()` narrowing IIRC

https://github.com/python/typeshed/pull/6302 was the PR that made the typeshed change -- the mypy_primer report is still up there if you want to take a look at the impact it had on the ecosystem as a whole 😄

---

_Comment by @sharkdp on 2025-10-13 14:34_

> > this specific case doesn't seem that compelling to me? Couldn't you use `hasattr` narrowing?
> 
> True, though I run pyright in CI as well as mypy, and IIRC pyright does not support `hasattr()` narrowing IIRC

The code above seems to be accepted by all type checkers

> [python/typeshed#6302](https://github.com/python/typeshed/pull/6302) was the PR that made the typeshed change -- the mypy_primer report is still up there if you want to take a look at the impact it had on the ecosystem as a whole 😄

I'm .... still not convinced 😃. Instead of modifying `ModuleType` for everyone (causing false negatives as well!), it feels like a similarly large ecosystem impact could have been achieved by modifying the return type of `builtins.__import__` /`importlib.import_module` to something more permissive (ideally `Any & ModuleType`, if we had intersections, or a dedicated `DynamicModule` type with the usual `ModuleType` attributes plus that dynamic `__getattr__` method).

In any case, that seems like a typeshed discussion. For ty, we should probably just follow typeshed and other typecheckers, I agree.

---

_Comment by @AlexWaygood on 2025-10-13 14:43_

> > > this specific case doesn't seem that compelling to me? Couldn't you use `hasattr` narrowing?
> > 
> > 
> > True, though I run pyright in CI as well as mypy, and IIRC pyright does not support `hasattr()` narrowing IIRC
> 
> The code above seems to be accepted by all type checkers

yes, because pyright/mypy/pyrefly respect `types.ModuleType.__getattr__` -- I was saying that this code would _not_ type-check with pyright if we removed `types.ModuleType.__getattr__` from typeshed, because pyright [doesn't support `hasattr()` narrowing](https://pyright-play.net/?pyrightVersion=1.1.405&strict=true&code=CYUwZgBA%2BgFAHgLggewEYCsQGMAuBKBAWACgIyIBLSACwEMBnWnHAJ3gBoIAiMZZLgiXLCIABxYUAdjngA6XsjwkgA)

> I'm .... still not convinced 😃. Instead of modifying `ModuleType` for everyone (causing false negatives as well!), it feels like a similarly large ecosystem impact could have been achieved by modifying the return type of `builtins.__import__` /`importlib.import_module` to something more permissive (ideally `Any & ModuleType`, if we had intersections, or a dedicated `DynamicModule` type with the usual `ModuleType` attributes plus that dynamic `__getattr__` method).

Hmm, I think it could cause problems if the type returned by `__import__` was not recognised as being the same class that module-literals from `import` statements are instances of. Using `Any & ModuleType` would obviously be the ideal solution here, but typeshed cannot use intersections yet 😢

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-10-17 14:43_

---

_Added to milestone `Beta` by @AlexWaygood on 2025-10-17 14:43_

---

_Unassigned @AlexWaygood by @AlexWaygood on 2025-10-17 15:01_

---

_Assigned to @sharkdp by @AlexWaygood on 2025-10-17 15:01_

---

_Closed by @sharkdp on 2025-11-14 12:59_

---
