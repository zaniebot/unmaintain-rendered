```yaml
number: 2225
title: Unexpected (to me) Divergent type manifesting in isinstance()
type: issue
state: closed
author: Wizzerinus
labels:
  - type aliases
assignees: []
created_at: 2025-12-26T09:26:10Z
updated_at: 2025-12-26T17:00:46Z
url: https://github.com/astral-sh/ty/issues/2225
synced_at: 2026-01-10T01:56:41Z
```

# Unexpected (to me) Divergent type manifesting in isinstance()

---

_Issue opened by @Wizzerinus on 2025-12-26 09:26_

### Summary

Consider the following example:

```py
class Foo: ...

if isinstance(123, "Foo"):
    pass
```

This example is obviously ill-formed, but what is interesting about it is the diagnostic that ty emits:

```
error[invalid-argument-type]: Argument to function `isinstance` is incorrect
 --> divergent.py:3:20
  |
1 | class Foo: ...
2 |
3 | if isinstance(123, "Foo"):
  |                    ^^^^^ Expected `type | UnionType | tuple[Divergent, ...]`, found `Literal["Foo"]`
4 |     pass
  |
info: Function defined here
    --> stdlib/builtins.pyi:3768:5
     |
3766 |     _ClassInfo: TypeAlias = type | tuple[_ClassInfo, ...]
3767 |
3768 | def isinstance(obj: object, class_or_tuple: _ClassInfo, /) -> bool:
     |     ^^^^^^^^^^              -------------------------- Parameter declared here
3769 |     """Return whether an object is an instance of a class or of a subclass thereof.
     |
info: rule `invalid-argument-type` is enabled by default
```

I am not sure the intention of this, but I would not expect a `Divergent` to appear in visible user output.

### Version

ty 0.0.7

---

_Label `type aliases` added by @mtshiba on 2025-12-26 11:55_

---

_Comment by @mtshiba on 2025-12-26 12:05_

Thanks for reporting this. It seems that the root issue is lack of support for self-referential generic implicit/PEP-613 type aliases (https://github.com/astral-sh/ty/issues/1738).



---

_Comment by @carljm on 2025-12-26 17:00_

In general, it is definitely possible for `Divergent` to appear in visible user output, in cases where there is recursion that doesn't converge.

In this particular case, this `Divergent` should go away once #1738 is fixed. The root cause is that the `_ClassInfo` type alias in typeshed is recursive.

---

_Closed by @carljm on 2025-12-26 17:00_

---
