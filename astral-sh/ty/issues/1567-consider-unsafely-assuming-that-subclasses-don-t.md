```yaml
number: 1567
title: "Consider unsafely assuming that subclasses don't add `__await__`, for better `isawaitable` narrowing"
type: issue
state: open
author: alexmacnorthstar
labels:
  - needs-decision
  - narrowing
assignees: []
created_at: 2025-11-14T21:40:24Z
updated_at: 2025-11-18T16:10:42Z
url: https://github.com/astral-sh/ty/issues/1567
synced_at: 2026-01-12T15:54:25Z
```

# Consider unsafely assuming that subclasses don't add `__await__`, for better `isawaitable` narrowing

---

_@alexmacnorthstar_

### Summary

```py
import asyncio
import inspect
from typing import Callable, Awaitable, Any

def foo_1():
  return [1,2,3]

async def foo_2():
  return [1,2,3]

async def test(
  foo: list[int] | Awaitable[list[int]]
):
  intlist = await foo if inspect.isawaitable(foo) else foo
  for x in intlist:
    print(x)

async def main():
  await test(foo_1())
  await test(foo_2())

asyncio.run(main())
```

Gives

```
error[not-iterable]: Object of type `object` is not iterable
  --> /Users/alex/Desktop/ty_test.py:15:12
   |
13 | ):
14 |   intlist = await foo if inspect.isawaitable(foo) else foo
15 |   for x in intlist:
   |            ^^^^^^^
16 |     print(x)
   |
info: It doesn't have an `__iter__` method or a `__getitem__` method
info: rule `not-iterable` is enabled by default

Found 1 diagnostic
```

The online playground shows that "intlist" is inferred as type "object" but it should be type "list[int]"

### Version

ty 0.0.1-alpha.26 (b225fd8b4 2025-11-10)

---

_Comment by @carljm on 2025-11-14 21:55_

This is another case where our narrowing is strictly correct, but perhaps overly pedantic.

It is possible that there exists some user subclass of `list` which is awaitable and returns some other type (not even necessarily the same type as contained in the list, so not necessarily `int` in this case). So after checking `inspect.isawaitable(foo)`, we can't assume that the type of `foo` is `Awaitable[list[int]]` -- all we can assume is that it is some awaitable object, and awaiting it returns... something. That's why we infer `object` as the type of `intlist`.

One path forward here could be to place some restrictions on subclassing certain builtin types. Like, emit a diagnostic if you subclass `list` and add an `__await__` method to it. Then we could safely narrow in the expected way here. But maybe somebody out there will find this restriction unbearable?

Or we could just decide to make some unsafe assumptions in these cases.

---

_Renamed from "type check with inspect.isawaitable does not appear to narrow type" to "Consider unsafely assuming that subclasses don't add `__await__`, for better `isawaitable` narrowing" by @carljm on 2025-11-14 21:56_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 21:56_

---

_Label `narrowing` added by @carljm on 2025-11-14 22:00_

---

_Label `needs-decision` added by @carljm on 2025-11-14 22:01_

---

_Comment by @alexmacnorthstar on 2025-11-14 22:19_

I would assume that subclassing list would be the edge case and I'd actually want a warning if someone did that in our codebase - but the real world is a full of crazy stuff, and i could imagine that in a more complex codebase like numpy that deals with interesting container types that they might do those kind of gymnastics...

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
