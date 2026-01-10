```yaml
number: 1814
title: Not inferring narrowing of type var when used in return type
type: issue
state: closed
author: hutchiko
labels:
  - narrowing
assignees: []
created_at: 2025-12-08T23:33:02Z
updated_at: 2025-12-09T00:48:48Z
url: https://github.com/astral-sh/ty/issues/1814
synced_at: 2026-01-10T01:56:41Z
```

# Not inferring narrowing of type var when used in return type

---

_Issue opened by @hutchiko on 2025-12-08 23:33_

### Summary

Type inference for type parameter does not appear to flow through to return type when narrowed via an `isinstance` check.

eg.

```python
def keep_type[T](value: T) -> T:
    if isinstance(value, str):
        return value.lower() 
    if isinstance(value, int):
        return 0
    return value
```

Fails with following errors at `return value.lower()` and `return 0` respectively.

```
Return type does not match returned value: expected `T@keep_type`, found `str`
```
```
Return type does not match returned value: expected `T@keep_type`, found `Literal[0]`
```




### Version

ty 0.0.1-alpha.32 (84a188116 2025-12-05)   

---

_Label `narrowing` added by @carljm on 2025-12-08 23:42_

---

_Comment by @carljm on 2025-12-08 23:48_

Hi, thanks for the report!

I think that ty is working as expected here, and the results are correct. Mypy and pyright emit the same diagnostics we do here.

Consider:

```py
class MyStr(str):
    pass

# According to the type signature of `keep_type`, we must reveal `MyStr` as the return type here.
# But at runtime, given the above definition of `keep_type`, this will return `str`, not `MyStr`. So
# the given implementation of `keep_type` is not sound.
reveal_type(keep_type(MyStr("foo")))

class MyInt(int):
    pass

# Similarly, the revealed type here should be `MyInt`, but the above implementation would return an instance of `int`,
# which is not sound.
reveal_type(keep_type(MyInt(1)))

```

---

_Closed by @carljm on 2025-12-08 23:48_

---

_Comment by @carljm on 2025-12-09 00:04_

We should support this pattern if the type var is constrained to `(int, str)` (meaning that even if the argument is a `MyStr` or a `MyInt`, we would still always solve the return type to `int` or `str`). That's tracked in https://github.com/astral-sh/ty/issues/621

---

_Comment by @hutchiko on 2025-12-09 00:07_

Thanks Carl. Makes sense. Appreciate the quick response. 

I would love to know how to write a function type signature that does work for my intent if possible.

I tried with strict type narrowing and the return type checks still fail. This should be sound for your example above, but maybe not for others?

```
def keep_type[T](value: T) -> T:
    if type(value) is str:
        return value.lower()
    if type(value) is int:
        return 0
    return value
```

---

_Comment by @carljm on 2025-12-09 00:23_

Yes, I think the version with `type(value) is str` is sound. It'd be a bit of a lift to support it, because it requires a new type variant to represent "instance of X, excluding subclasses", which is not a type we currently have a representation for.

I think if the semantics you want are those shown in your latest version, currently your simplest approach would be to write that version and just use a couple type-ignores and some comments to explain why it's sound.

---

_Comment by @AlexWaygood on 2025-12-09 00:30_

You can consider using overloads:

```py
from typing import overload

@overload
def keep_type(value: str) -> str: ...

@overload
def keep_type(value: int) -> int: ...

@overload
def keep_type[T](value: T) -> T: ...

def keep_type[T](value: str | int | T) -> str | int | T:
    if isinstance(value, str):
        return value.lower() 
    if isinstance(value, int):
        return 0
    return value
```

---

_Comment by @hutchiko on 2025-12-09 00:48_

Thanks all for such helpful responses. Much appreciated. 

---
