```yaml
number: 483
title: "Incorrect `invalid-return-type` in type-preserving generic mapping function branching on runtime type"
type: issue
state: open
author: shilch
labels:
  - wish
  - generics
assignees: []
created_at: 2025-05-22T08:03:34Z
updated_at: 2025-11-18T16:10:28Z
url: https://github.com/astral-sh/ty/issues/483
synced_at: 2026-01-10T01:58:59Z
```

# Incorrect `invalid-return-type` in type-preserving generic mapping function branching on runtime type

---

_Issue opened by @shilch on 2025-05-22 08:03_

### Summary

Following function is flagged incorrectly(?) my ty:

```python
def func[T](val: T) -> T:
    if type(val) is str:
        return ''
    return val
```

The following output is generated:

```
error[invalid-return-type]: Return type does not match returned value
 --> test.py:1:24
  |
1 | def func[T](val: T) -> T:
  |                        - Expected `T` because of return type
2 |     if type(val) is str:
3 |         return ''
  |                ^^ expected `T`, found `Literal[""]`
4 |     return val
  |
info: rule `invalid-return-type` is enabled by default
```

This behavior is consistent with mypy but looks erroneous: https://github.com/python/mypy/issues/12989#issuecomment-1691226161

### Version

ty 0.0.1-alpha.6

---

_Label `wish` added by @carljm on 2025-05-22 14:27_

---

_Label `generics` added by @carljm on 2025-05-22 14:27_

---

_Comment by @carljm on 2025-05-22 14:34_

Thanks for the report! Our behavior here is consistent with both mypy and pyright.

I think the requested behavior here is safe only if you do a "type(...) is" check (not e.g. an `isinstance` check which would permit subclasses). (This means it would also require that we add support for "exact" instance types which exclude subclasses.)

It also requires ensuring that a generic function typevar can never be solved to a more precise type than what can be determined at runtime via `type(...) is`. For example, a typevar can never be solved to a literal type (currently we will). This also wouldn't work in any case involving a generic type instead of `str`, since generics are erased at runtime and thus can't be checked via `type(...) is`.

All in all, I don't think it's out of the question that we might someday consider this, but I'm not sure we will ever support it, and it's certainly low priority for now.

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-15 01:58_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
