```yaml
number: 441
title: Attributes of types.SimpleNamespace not resolved
type: issue
state: closed
author: squingo44
labels:
  - bug
  - help wanted
  - attribute access
assignees: []
created_at: 2025-05-18T14:11:44Z
updated_at: 2025-05-26T10:59:46Z
url: https://github.com/astral-sh/ty/issues/441
synced_at: 2026-01-10T02:34:10Z
```

# Attributes of types.SimpleNamespace not resolved

---

_Issue opened by @squingo44 on 2025-05-18 14:11_

### Summary

Ty can't resolve custom SimpleNamespace attributes. Minimal repro:

```python
from types import SimpleNamespace

sn = SimpleNamespace(a="a")

a = sn.a
```
Running `ty check` with no configuration:
```
error[unresolved-attribute]: Type `SimpleNamespace` has no attribute `a`
 --> test_simplenamespace_ty.py:5:5
  |
3 | sn = SimpleNamespace(a="a")
4 |
5 | a = sn.a
  |     ^^^^
  |
info: rule `unresolved-attribute` is enabled by default
```

### Version

0.0.1-alpha.5 (3b726d87a 2025-05-17)

---

_Renamed from "Attributes of types.SimpleNamespace not resolved correctly" to "Attributes of types.SimpleNamespace not resolved" by @squingo44 on 2025-05-18 14:12_

---

_Label `bug` added by @AlexWaygood on 2025-05-18 14:12_

---

_Label `attribute access` added by @AlexWaygood on 2025-05-18 14:12_

---

_Comment by @AlexWaygood on 2025-05-18 14:15_

It looks like we're not respecting the custom `__getattribute__` method on `SimpleNamespace` here. We already handle custom `__getattr__` methods, but apparently not `__getattribute__` methods.

Mypy and pyright treat `__getattribute__` methods exactly the same way as `__getattr__` methods. That's not *exactly* faithful to the runtime semantics, but pragmatically the best thing for us to do might be just to follow their precedent here.

---

_Comment by @squingo44 on 2025-05-18 19:55_

What's the difference in the runtime semantics?

---

_Comment by @AlexWaygood on 2025-05-18 20:07_

Compare and contrast:

```pycon
>>> class Foo:
...     x = 42
...     def __getattribute__(self, attr):
...         return "x"
...         
>>> class Bar:
...     x = 42
...     def __getattr__(self, attr):
...         return "x"
...         
>>> Foo().x
'x'
>>> Foo().y
'x'
>>> Bar().x
42
>>> Bar().y
'x'
```

So at runtime, accessing _any_ attribute on `Foo` returns the object returned by `__getattribute__`, even if it can be found at the usual locations for attribute access (class dictionaries, instance dictionaries, etc.). But for `Bar`, `__getattr__` is _only_ called if the attribute cannot be found at any of the "usual locations" -- the method is used as a fallback rather than a total override of normal attribute lookup.

These are not the semantics mypy/pyright implement -- mypy/pyright treat `__getattribute__` as if it worked exactly the same way as `__getattr__`. For example, here mypy and pyright both reveal `str` for the `x` attribute on an instance of `Foo`, even though `__getattribute__` is present and annotated as always returning an `int` (and `__getattribute__` at runtime totally overrides all the normal attribute-lookup machinery).

```py
class Foo:
    x: str
    def __getattribute__(self, attr: str) -> int:
        return 42

reveal_type(Foo().x)  # str!
```

- https://mypy-play.net/?mypy=latest&python=3.12&gist=d6fc5cb2e245e47ab53db4403e7bed2b
- https://pyright-play.net/?pythonVersion=3.12&strict=true&enableExperimentalFeatures=true&code=MYGwhgzhAEBiD28BcAoa7oA8nQgFwCc0MATAUwDNoB9agczLzD0IEsAjAVzzNoAoIZEBQA00ZoRz4CASmgBaAHzRWAOzyoMW6AUacCq6ABYATChS6AbmTAhqeAJ4AHMnwTw%2BMgHSY50AMS4hACEKEA

---

_Comment by @carljm on 2025-05-19 02:45_

Do we know _why_ mypy/pyright depart from the runtime semantics in this way? Is there some pragmatic rationale for real world use cases? It doesn’t seem particularly hard to implement the real semantics. I guess one possible rationale is that it might be expensive to have to check every class on every attribute access for a `__getattribute__` method. 

---

_Comment by @JelleZijlstra on 2025-05-19 16:00_

Note that `builtins.object` has a `__getattribute__` method in typeshed, so a naive implementation might have some interesting results. (I assume other type checkers have a special case to ignore `object.__getattribute__` in some circumstances?)

A consequence of implementing the "correct" semantics for `__getattribute__` is that it would override access to attributes defined on base classes, so if you have a `__getattribute__`, its return type must be Any or you're violating LSP.



---

_Comment by @AlexWaygood on 2025-05-19 16:03_

> Do we know _why_ mypy/pyright depart from the runtime semantics in this way?

You'd [have to ask](https://github.com/python/mypy/pull/2705) @JelleZijlstra ;)

---

_Comment by @AlexWaygood on 2025-05-19 16:07_

We discussed this in person at the sprints and we think we should just follow mypy/pyright here and treat it as if it has the same semantics as `__getattr__`. If a class looks like this:

```py
class Foo:
    x: str
    def __getattribute__(self, attr) -> int: ...
```

then it seems pretty obvious that they want the type checker to understand that accessing the `x` attribute on instances of `Foo` should resolve to a `str` type. There's no other reason why they would have added that annotation. Pragmatically, treating it as if it had the same semantics as `__getattr__` seems like the behaviour that's most likely to reflect the user's intent when writing the class.

---

_Label `help wanted` added by @AlexWaygood on 2025-05-19 16:07_

---

_Comment by @JelleZijlstra on 2025-05-19 16:08_

I guess JelleZijlstra from 2017 was way ahead of you even if JelleZijlstra from 2025 has no memory of this :)

---

_Closed by @sharkdp on 2025-05-26 10:59_

---
