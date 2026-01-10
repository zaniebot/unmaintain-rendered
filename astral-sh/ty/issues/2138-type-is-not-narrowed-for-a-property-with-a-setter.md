---
number: 2138
title: Type is not narrowed for a property with a setter
type: issue
state: closed
author: goodspark
labels:
  - narrowing
assignees: []
created_at: 2025-12-21T00:09:19Z
updated_at: 2025-12-22T20:08:21Z
url: https://github.com/astral-sh/ty/issues/2138
synced_at: 2026-01-10T01:52:52Z
---

# Type is not narrowed for a property with a setter

---

_Issue opened by @goodspark on 2025-12-21 00:09_

### Summary

Hi all,
I ran into a case where a type didn't seem to get narrowed because its property is technically stating a superset of types. Minimal example: https://play.ty.dev/ab00ad10-93c6-4dee-b0ac-555f6c5688be

```
error[invalid-assignment]: Cannot assign to a subscript on an object of type `None`
  --> asd.py:17:1
   |
15 | if a.b is None:
16 |     a.b = {}
17 | a.b["asd"] = True
   | ^^^^^^^^^^
   |
info: The full type of the subscripted object is `dict[Unknown, Unknown] | None`
info: `None` does not have a `__setitem__` method.
```

No special ty configuration.

FWIW pyright seems happy with this as long as `a.b` is set in the line above. This happens for other objects/mutable types as well. Thinking about it more, I do think ty is being more technically correct here (ex. what if something in the setter decided the given value was not valid and didn't actually change the internal value of `_b`, as shown below?) but it leads to this odd detection.

```py
    @b.setter
    def b(self, val: dict | None):
        if val is not None and len(val) == 0:
            return
        self._b = val
```

I did a search for 'type narrowing property' and saw this issue: https://github.com/astral-sh/ty/issues/628
It seems related but it's specifically talking about generics, which aren't being used here. So I decided to create a new issue. Feel free to merge/close-as-duplicate if it's actually the same issue!

### Version

0.0.5

---

_Label `narrowing` added by @mtshiba on 2025-12-21 05:08_

---

_Comment by @carljm on 2025-12-22 20:08_

Thanks for the report! This is indeed the same as #628. The mention of generics there is just discussing one possible heuristic that could be used to decide whether it's safe to narrow a descriptor attribute or not. The issue itself is the same: ty currently (intentionally) doesn't ever narrow descriptors, but maybe we should for compatibility.

---

_Closed by @carljm on 2025-12-22 20:08_

---
