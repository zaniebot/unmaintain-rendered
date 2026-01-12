```yaml
number: 479
title: Spurious possibly-unbound-attribute
type: issue
state: closed
author: danielhollas
labels:
  - narrowing
assignees: []
created_at: 2025-05-21T20:46:54Z
updated_at: 2025-05-21T20:57:31Z
url: https://github.com/astral-sh/ty/issues/479
synced_at: 2026-01-12T15:54:23Z
```

# Spurious possibly-unbound-attribute

---

_@danielhollas_

### Summary

Reproducer

```python
class Context:

    def __init__(self, default_map):
        self.default_map: dict | None = default_map

def test(
    ctx: Context,
):
    ctx.default_map = ctx.default_map or {}
    a = {'a': 1}
    ctx.default_map.update(a)
```

```console
> ty check test.py
warning[possibly-unbound-attribute]: Attribute `update` on type `dict[Unknown, Unknown] | None` is possibly unbound
  --> test.py:12:5
   |
10 |     ctx.default_map = ctx.default_map or {}
11 |     a = {'a': 1}
12 |     ctx.default_map.update(a)
   |     ^^^^^^^^^^^^^^^^^^^^^^
   |
info: rule `possibly-unbound-attribute` is enabled by default

Found 1 diagnostic
```

https://play.ty.dev/19b86451-c3e0-455c-a3d7-78b623def6ab

It looks like the declared type is preferred over the narrowed type? If I remove the type annotation in the `__init__` method the error goes away.  

### Version

0.0.1-alpha.6

---

_Closed by @AlexWaygood on 2025-05-21 20:48_

---

_Comment by @AlexWaygood on 2025-05-21 20:49_

Thanks! We don't narrow the types of attributes at all yet, but we're working on it!

---

_Label `narrowing` added by @AlexWaygood on 2025-05-21 20:49_

---

_Comment by @danielhollas on 2025-05-21 20:57_

Ah, sorry, I was aware of that issue, but thought this is something different. I got confused why the error went away when I removed the type, but now I understand that in that case the inferred type was Unknown, and so narrowing was not necessary. Sorry for the noise! 

---
