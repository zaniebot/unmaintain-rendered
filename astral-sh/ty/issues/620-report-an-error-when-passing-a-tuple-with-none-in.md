```yaml
number: 620
title: "report an error when passing a tuple with `None` in an `isinstance` check"
type: issue
state: closed
author: DetachHead
labels: []
assignees: []
created_at: 2025-06-10T10:47:22Z
updated_at: 2025-11-21T00:08:09Z
url: https://github.com/astral-sh/ty/issues/620
synced_at: 2026-01-12T15:54:23Z
```

# report an error when passing a tuple with `None` in an `isinstance` check

---

_@DetachHead_

```py
if isinstance(None, (int, None)): # no error, crashes at runtime
    ...
```
```
TypeError: isinstance() arg 2 must be a type, a tuple of types, or a union
```
https://play.ty.dev/678e67c5-5f1c-4221-aa90-ca9ed58d1136

confusingly, it's valid if you use union syntax instead, but that's not supported by ty yet (#122):

```py
if isinstance(None, int | None):
    ...
```

---

_Comment by @AlexWaygood on 2025-06-10 12:15_

You can pass literally anything as the second argument to `isinstance()` right now, and ty won't complain 😆 https://play.ty.dev/998df323-c622-4ac8-8185-6f022627cd4e

The reason is that typeshed [uses an old-style type alias](https://github.com/python/typeshed/blob/0db0486fe6a0eb6b4ebf1256fe0afe0b95e2dc36/stdlib/builtins.pyi#L1527-L1533) for the annotation of the second parameter, and we don't support those yet -- so we incorrectly infer that the second parameter is annotated with `Any`, and allow all types to be passed into that parameter.

In general, ty [is aware](https://play.ty.dev/f386d0d9-c50e-469a-85ea-ceaf492243ae) that `None` is not an instance of `type` (we special-case `None` much less than some other type checkers), so I expect this issue to resolve itself naturally when we add support for type aliases.

---

_Closed by @AlexWaygood on 2025-06-10 12:15_

---

_Comment by @carljm on 2025-11-21 00:08_

This will not be fixed by https://github.com/astral-sh/ruff/pull/21394 because it also depends on full support for recursive type aliases; this is tracked in https://github.com/astral-sh/ty/issues/1604

---
