```yaml
number: 589
title: "traversal of 'Never' branches"
type: issue
state: closed
author: lczyk
labels: []
assignees: []
created_at: 2025-06-05T23:34:07Z
updated_at: 2025-06-06T00:07:59Z
url: https://github.com/astral-sh/ty/issues/589
synced_at: 2026-01-12T15:54:23Z
```

# traversal of 'Never' branches

---

_@lczyk_

### Summary

one more based on [type_conversion_dict](https://github.com/MarcinKonowalczyk/type_conversion_dict). There is a helper function for converting to a custom dictionary type. here is a mwe to reproduce

```python
from typing import Union
from typing_extensions import reveal_type

class MyDict(dict):
    pass


# These are not needed for the example, but the actual function is overloaded.

# from typing import Any, overload
# @overload
# def nested_convert(d: dict) -> MyDict: ...
# @overload
# def nested_convert(d: list) -> list: ...
# @overload
# def nested_convert(d: Any, *, _depth: int) -> Any: ...


def nested_convert(d: Union[dict, list], *, _depth: int = 0) -> Union[MyDict, list]:
    """Convert a nested dict to MyDict."""

    if isinstance(d, dict):
        kv = ((k, nested_convert(v, _depth=_depth + 1)) for k, v in d.items())
        return MyDict(kv)
    elif isinstance(d, list):
        vs = (nested_convert(v, _depth=_depth + 1) for v in d)
        return list(vs)
    else:
        if _depth == 0:
            raise TypeError("Input must be a dict or a list")
        reveal_type(d)
        return d

```

which gives

```
error[invalid-return-type] mwe.py:31:16: Return type does not match returned value: expected `MyDict | list[Unknown]`, found `(dict[Unknown, Unknown] & ~dict[Unknown, Unknown] & ~list[Unknown]) | (list[Unknown] & ~dict[Unknown, Unknown] & ~list[Unknown])`
```

but works in mypy.

i'm unsure whether this is a correct behaviour of mypy...? mypy does not show any output for `reveal_type` and the language server reports `d`'s type (in the last `return d` line) as `Never`, presumably since that branch is never taken by mypy.

This is definitely a bit more of a fringe example and can also be kinda solved with `@no_type_check` decorator (since the actual implementation has a bunch of overloads on top of it to provide the interface. It's hopefully an interesting example though.


### Version

ty 0.0.1-alpha.8 (c1337c962 2025-06-02)

---

_Comment by @carljm on 2025-06-05 23:36_

I think this is a nice example for #456 

---

_Closed by @carljm on 2025-06-05 23:36_

---

_Comment by @lczyk on 2025-06-05 23:38_

hmm... i did see that thread but it did not seem like the same thing. i will have a more thorough read! ty :))

---
