---
number: 2237
title: "io.TextIOWrapper: \"Definition is incompatible with `IO.write`\""
type: issue
state: closed
author: bastimeyer
labels: []
assignees: []
created_at: 2025-12-27T16:13:45Z
updated_at: 2025-12-27T19:08:59Z
url: https://github.com/astral-sh/ty/issues/2237
synced_at: 2026-01-10T01:51:14Z
---

# io.TextIOWrapper: "Definition is incompatible with `IO.write`"

---

_Issue opened by @bastimeyer on 2025-12-27 16:13_

### Summary

I couldn't find anything with `TextIOWrapper` on the issue tracker. I've also checked the pinned thread with the status of various implementations, but I'm not 100% sure if this is covered yet (#1714 ?!), so I decided to post this new issue.

`TextIOWrapper`'s typing data is defined like this in typeshed (notice the comment):
```py
class TextIOWrapper(TextIOBase, _TextIOBase, TextIO, Generic[_BufferT_co]):  # type: ignore[misc]  # incompatible definitions of write in the base classes
```
https://github.com/python/typeshed/blob/6b4691d04491dc3800fef9598c526f49c7fa4bd7/stdlib/_io.pyi#L239-L272

with its `write(self, s: str, /) -> int: ...` method being inherited from `_TextIOBase`:
https://github.com/python/typeshed/blob/6b4691d04491dc3800fef9598c526f49c7fa4bd7/stdlib/_io.pyi#L201-L212

The `_BufferT_co` typevar, which is bound to `_WrappedBuffer(Protocol)` however defines `def write(self, b: bytes, /) -> object: ...`:
https://github.com/python/typeshed/blob/6b4691d04491dc3800fef9598c526f49c7fa4bd7/stdlib/_io.pyi#L214-L237

----

Example:

```py
from io import BytesIO, TextIOWrapper


class MyTextIOWrapper(TextIOWrapper):
    def write(self, s: str) -> int:
        data = s.encode("utf-8")
        self.buffer.write(data)

        return len(data)


buffer = BytesIO()
inst = MyTextIOWrapper(buffer)
inst.write("foo")

assert buffer.getvalue() == b"foo"
```

```
$ ty check scratch_1.py 
error[invalid-method-override]: Invalid override of method `write`
    --> scratch_1.py:5:9
     |
   4 | class MyTextIOWrapper(TextIOWrapper):
   5 |     def write(self, s: str) -> int:
     |         ^^^^^^^^^^^^^^^^^^^^^^^^^^ Definition is incompatible with `IO.write`
   6 |         data = s.encode("utf-8")
   7 |         self.buffer.write(data)
     |
    ::: stdlib/typing.pyi:1427:9
     |
1425 |     @abstractmethod
1426 |     @overload
1427 |     def write(self, s: AnyStr, /) -> int: ...
     |         -------------------------------- `IO.write` defined here
1428 |     @abstractmethod
1429 |     @overload
     |
info: This violates the Liskov Substitution Principle
info: rule `invalid-method-override` is enabled by default

Found 1 diagnostic
```

https://play.ty.dev/58ec3283-eb45-430e-8719-1d18c6003f7a

### Version

0.0.7

---

_Comment by @carljm on 2025-12-27 19:08_

Thanks for the report! This is another example of #2000 -- I'll close as duplicate, but mention on that issue that this is another case in the standard library.

---

_Closed by @carljm on 2025-12-27 19:08_

---
