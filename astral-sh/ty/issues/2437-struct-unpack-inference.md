```yaml
number: 2437
title: Struct unpack inference
type: issue
state: open
author: sakgoyal
labels:
  - question
assignees: []
created_at: 2026-01-10T22:44:38Z
updated_at: 2026-01-12T06:35:06Z
url: https://github.com/astral-sh/ty/issues/2437
synced_at: 2026-01-12T06:54:49Z
```

# Struct unpack inference

---

_Issue opened by @sakgoyal on 2026-01-10 22:44_

### Question

Can the type checker support introspecting the `struct.unpack` format string to infer the return type? currently it returns `Any`, but i'd like it to return a specified type if possible (when a literal string is provided). 

I am trying to add types to an untyped library, but it uses a lot of struct unpacking which results in a lot of manual casts or Any's everywhere. 

I already wrote some python code that seems to get the type at runtime. but it's just a PoC to show that it's possible to do. 

```py
import re
from typing import cast

type TypeMap = type[int | bool | str | bytes | float]

_TYPE_MAP: dict[str, TypeMap] = {c: int for c in 'bBhHiIlLqQnNP'}
_TYPE_MAP.update(dict.fromkeys('efd', float))
_TYPE_MAP.update(dict.fromkeys('c', bytes))
_TYPE_MAP['?'] = bool

def get_unpack_type(fmt: str):
    core = fmt.lstrip('@=<>!')

    types: list[TypeMap] = []
    for count, tag in cast(list[tuple[str, str]], re.findall(r'(\d*)([xcbB?hHiIlLqQnNefdspP])', core)):
        if tag == 'x':
            continue
        if tag in 'sp':
            types.append(bytes)
        else:
            t = _TYPE_MAP.get(tag, int)
            n = int(count) if count else 1
            types.extend([t] * n)
    if len(types) == 1 and 'x' not in core:
        return types[0]
    return tuple[tuple(types)]


if __name__ == '__main__':
    test_cases = [
        ('2xH', tuple[int]),
        ('@i', int),
        ('=i', int),
        ('c3x', tuple[bytes]),
        ('2c', tuple[bytes, bytes]),
        ('5c', tuple[bytes, bytes, bytes, bytes, bytes]),
        ('0s', bytes),
        ('1s', bytes),
        ('255s', bytes),
        ('e', float),
        ('2e', tuple[float, float]),
        ('e4x', tuple[float]),
        ('3eH', tuple[float, float, float, int]),
        ('?x?', tuple[bool, bool]),
        ('2?', tuple[bool, bool]),
        ('?2xI', tuple[bool, int]),
        ('fd4x', tuple[float, float]),
        ('d2xH', tuple[float, int]),
        ('2i4x2h', tuple[int, int, int, int]),
        ('iP', tuple[int, int]),
        ('>n2xN', tuple[int, int]),
        ('!P4x', tuple[int]),
    ]

```

### Version

_No response_

---

_Label `question` added by @sakgoyal on 2026-01-10 22:44_

---

_Comment by @dhruvmanila on 2026-01-12 06:32_

I don't think so, mainly because the type of `struct.unpack` in typeshed is:

```py
def unpack(format: str | bytes, buffer: ReadableBuffer, /) -> tuple[Any, ...]:
```

> I am trying to add types to an untyped library, but it uses a lot of struct unpacking which results in a lot of manual casts or Any's everywhere.

Can you provide a small example of this case? I don't see any `struct.unpack` in the provided code snippet.

---

_Comment by @dhruvmanila on 2026-01-12 06:35_

Oh, is `get_unpack_type` an equivalent implementation of `struct.unpack` ?

---
