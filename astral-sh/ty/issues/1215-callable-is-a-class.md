```yaml
number: 1215
title: "`Callable` is a class"
type: issue
state: closed
author: KotlinIsland
labels: []
assignees: []
created_at: 2025-09-20T07:09:18Z
updated_at: 2025-09-22T08:34:11Z
url: https://github.com/astral-sh/ty/issues/1215
synced_at: 2026-01-10T02:06:25Z
```

# `Callable` is a class

---

_Issue opened by @KotlinIsland on 2025-09-20 07:09_

### Summary

```py
from collections.abc import Callable

t: type = Callable  # Object of type `typing.Callable` is not assignable to `type`
```

this stems from an issue with typeshed

---

_Comment by @AlexWaygood on 2025-09-20 11:33_

This is only true for `collections.abc.Callable`, not for `typing.Callable`:

```pycon
>>> import collections.abc, typing
>>> isinstance(collections.abc.Callable, type)
True
>>> isinstance(typing.Callable, type)
False
>>> collections.abc.Callable.__mro__
(<class 'collections.abc.Callable'>, <class 'object'>)
>>> typing.Callable.__mro__
Traceback (most recent call last):
  File "<python-input-4>", line 1, in <module>
    typing.Callable.__mro__
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 1366, in __getattr__
    raise AttributeError(attr)
AttributeError: __mro__. Did you mean: '__or__'?
```

According to typeshed, `typing.Callable` and `collections.abc.Callable` are the same object, and it would require quite deep special casing from us to override typeshed on that point. So I'm not sure this is worth fixing.

---

_Comment by @KotlinIsland on 2025-09-20 15:11_

hmmm
```py
from collections.abc import Callable

class A(Callable[int]): # no error
    pass
```

- #1220

---

_Comment by @KotlinIsland on 2025-09-21 03:55_

@AlexWaygood would it be worth updating the vendored typeshed? if so i'd be happy to do it

---

_Comment by @AlexWaygood on 2025-09-22 08:07_

> [@AlexWaygood](https://github.com/AlexWaygood) would it be worth updating the vendored typeshed? if so i'd be happy to do it

I think that would be likely to either break existing type checkers, or else have somewhat unexpected effects on their handling of `Callable`. I'm not sure it's worth the disruption to the ecosystem.

Is there real-world code where this causes issues? What's the use case needing to consider `collections.abc.Callable` (but not `typing.Callable`) an instance of `type` 

---

_Comment by @KotlinIsland on 2025-09-22 08:10_

> I think that would be likely to either break existing type checkers 

i mean, ty's vendored typeshed, not the upstream one

> Is there real-world code where this causes issues?

i have run into it in the past where the `typing` definition of `Callable` has caused issues, i think there is some special handling in ty already to avoid this as `class C(Callable)` isn't reported. imo it's better to iron out this incorrect definition at the root

---

_Comment by @AlexWaygood on 2025-09-22 08:18_

> i mean, ty's vendored typeshed, not the upstream one

I see — no, I'd strongly prefer it if we can avoid patching upstream typeshed unless we have a very good reason for it. The patches mypy has applied to typeshed have been a real maintenance headache for them and made syncing typeshed much more complicated for them than it should be because of the frequent merge conflicts. I don't want for us to get into a similar situation.

Our handling of special forms is quite different to most other type checkers, as you've already observed, so I think it's unlikely that we'll have the same bugs that you've observed with mypy. https://github.com/astral-sh/ty/issues/1220 relates to a branch of code in ty that is already marked as TODO. We actually infer a `Todo` type currently (which is similar to `Unknown`, but is explicit about the fact that there's still some missing logic in ty) when `Callable` is present as a base class.

So all in all, I don't see sufficient motivation to make any changes with regards to this one. But thanks for the report!

---

_Closed by @AlexWaygood on 2025-09-22 08:18_

---

_Comment by @KotlinIsland on 2025-09-22 08:34_

okay, i agree

i've opened: https://github.com/python/typeshed/pull/14761

---
