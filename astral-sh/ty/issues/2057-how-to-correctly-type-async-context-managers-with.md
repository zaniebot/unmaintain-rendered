```yaml
number: 2057
title: How to correctly type async context managers with overloads
type: issue
state: open
author: ddanier
labels:
  - bug
  - calls
  - overloads
assignees: []
created_at: 2025-12-18T10:12:53Z
updated_at: 2026-01-12T08:29:55Z
url: https://github.com/astral-sh/ty/issues/2057
synced_at: 2026-01-12T15:54:26Z
```

# How to correctly type async context managers with overloads

---

_@ddanier_

### Question

I'm kind of desperate on this one, as nothing seems to work. I have an async context manager using some overloads. Nay combination I so far tried to use fails the `ty` type check. See the following code for examples:

```python
from collections.abc import AsyncIterator
from contextlib import asynccontextmanager
from typing import AsyncContextManager, overload, reveal_type

# Not using overloads - works as expected

@asynccontextmanager
async def test_contextmanager_plain(val: str | int) -> AsyncIterator[str]:
    yield "hello world"

reveal_type(test_contextmanager_plain)

async def call_asynccontextmanager_plain():
    async with test_contextmanager_plain(123) as x:
        reveal_type(x)

# Using async context manager types

@overload
def test_contextmanager_using_types(val: str) -> AsyncContextManager[str]: ...
@overload
def test_contextmanager_using_types(val: int) -> AsyncContextManager[str]: ...
@asynccontextmanager
async def test_contextmanager_using_types(val: str | int) -> AsyncIterator[str]:
    yield "hello world"

reveal_type(test_contextmanager_using_types)

async def call_asynccontextmanager_using_types():
    async with test_contextmanager_using_types(123) as x:
        reveal_type(x)

# Using async iterator types (although this sounds wrong)

@overload
def test_contextmanager_using_wrong_types(val: str) -> AsyncIterator[str]: ...
@overload
def test_contextmanager_using_wrong_types(val: int) -> AsyncIterator[str]: ...
@asynccontextmanager
async def test_contextmanager_using_wrong_types(val: str | int) -> AsyncIterator[str]:
    yield "hello world"

reveal_type(test_contextmanager_using_wrong_types)

async def call_asynccontextmanager_using_wrong_types():
    async with test_contextmanager_using_wrong_types(123) as x:
        reveal_type(x)

# # Using the decorator in overloads

@overload
@asynccontextmanager
async def test_contextmanager_with_decorator(val: str) -> AsyncIterator[str]: ...
@overload
@asynccontextmanager
async def test_contextmanager_with_decorator(val: int) -> AsyncIterator[str]: ...
@asynccontextmanager
async def test_contextmanager_with_decorator(val: str | int) -> AsyncIterator[str]:
    yield "hello world"

reveal_type(test_contextmanager_with_decorator)

async def call_asynccontextmanager_with_decorator():
    async with test_contextmanager_with_decorator(123) as x:
        reveal_type(x)
```

The `ty` result is:
```
info[revealed-type]: Revealed type
  --> ty_test.py:11:13
   |
 9 |     yield "hello world"
10 |
11 | reveal_type(test_contextmanager_plain)
   |             ^^^^^^^^^^^^^^^^^^^^^^^^^ `(val: str | int) -> _AsyncGeneratorContextManager[str, None]`
12 |
13 | async def call_asynccontextmanager_plain():
   |

info[revealed-type]: Revealed type
  --> ty_test.py:15:21
   |
13 | async def call_asynccontextmanager_plain():
14 |     async with test_contextmanager_plain(123) as x:
15 |         reveal_type(x)
   |                     ^ `str`
16 |
17 | # Using async context manager types
   |

error[invalid-argument-type]: Argument to function `asynccontextmanager` is incorrect
  --> ty_test.py:23:1
   |
21 | @overload
22 | def test_contextmanager_using_types(val: int) -> AsyncContextManager[str]: ...
23 | @asynccontextmanager
   | ^^^^^^^^^^^^^^^^^^^^ Expected `(...) -> AsyncIterator[Unknown]`, found `Overload[(val: str) -> AbstractAsyncContextManager[str, bool | None], (val: int) -> AbstractAsyncContextManager[str, bool | None]]`
24 | async def test_contextmanager_using_types(val: str | int) -> AsyncIterator[str]:
25 |     yield "hello world"
   |
info: Function defined here
   --> stdlib/contextlib.pyi:177:5
    |
175 |         ) -> bool | None: ...
176 |
177 | def asynccontextmanager(func: Callable[_P, AsyncIterator[_T_co]]) -> Callable[_P, _AsyncGeneratorContextManager[_T_co]]:
    |     ^^^^^^^^^^^^^^^^^^^ ---------------------------------------- Parameter declared here
178 |     """@asynccontextmanager decorator.
    |
info: rule `invalid-argument-type` is enabled by default

info[revealed-type]: Revealed type
  --> ty_test.py:27:13
   |
25 |     yield "hello world"
26 |
27 | reveal_type(test_contextmanager_using_types)
   |             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ `(...) -> _AsyncGeneratorContextManager[Unknown, None]`
28 |
29 | async def call_asynccontextmanager_using_types():
   |

info[revealed-type]: Revealed type
  --> ty_test.py:31:21
   |
29 | async def call_asynccontextmanager_using_types():
30 |     async with test_contextmanager_using_types(123) as x:
31 |         reveal_type(x)
   |                     ^ `str | Unknown`
32 |
33 | # Using async iterator types (although this sounds wrong)
   |

info[revealed-type]: Revealed type
  --> ty_test.py:43:13
   |
41 |     yield "hello world"
42 |
43 | reveal_type(test_contextmanager_using_wrong_types)
   |             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ `Overload[(val: str) -> _AsyncGeneratorContextManager[str | None, None], (val: int) -> _AsyncGeneratorContextManager[str | None, None]]`
44 |
45 | async def call_asynccontextmanager_using_wrong_types():
   |

error[invalid-context-manager]: Object of type `AsyncIterator[str] | _AsyncGeneratorContextManager[str | None, None]` cannot be used with `async with` because the methods `__aenter__` and `__aexit__` are possibly missing
  --> ty_test.py:46:16
   |
45 | async def call_asynccontextmanager_using_wrong_types():
46 |     async with test_contextmanager_using_wrong_types(123) as x:
   |                ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
47 |         reveal_type(x)
   |
info: rule `invalid-context-manager` is enabled by default

info[revealed-type]: Revealed type
  --> ty_test.py:47:21
   |
45 | async def call_asynccontextmanager_using_wrong_types():
46 |     async with test_contextmanager_using_wrong_types(123) as x:
47 |         reveal_type(x)
   |                     ^ `CoroutineType[Any, Any, str | None]`
48 |
49 | # # Using the decorator in overloads
   |

error[invalid-argument-type]: Argument to function `asynccontextmanager` is incorrect
  --> ty_test.py:52:1
   |
51 | @overload
52 | @asynccontextmanager
   | ^^^^^^^^^^^^^^^^^^^^ Expected `(...) -> AsyncIterator[Unknown]`, found `def test_contextmanager_with_decorator(val: str) -> CoroutineType[Any, Any, AsyncIterator[str]]`
53 | async def test_contextmanager_with_decorator(val: str) -> AsyncIterator[str]: ...
54 | @overload
   |
info: Function defined here
   --> stdlib/contextlib.pyi:177:5
    |
175 |         ) -> bool | None: ...
176 |
177 | def asynccontextmanager(func: Callable[_P, AsyncIterator[_T_co]]) -> Callable[_P, _AsyncGeneratorContextManager[_T_co]]:
    |     ^^^^^^^^^^^^^^^^^^^ ---------------------------------------- Parameter declared here
178 |     """@asynccontextmanager decorator.
    |
info: rule `invalid-argument-type` is enabled by default

error[invalid-argument-type]: Argument to function `asynccontextmanager` is incorrect
  --> ty_test.py:55:1
   |
53 | async def test_contextmanager_with_decorator(val: str) -> AsyncIterator[str]: ...
54 | @overload
55 | @asynccontextmanager
   | ^^^^^^^^^^^^^^^^^^^^ Expected `(...) -> AsyncIterator[Unknown]`, found `def test_contextmanager_with_decorator(val: int) -> CoroutineType[Any, Any, AsyncIterator[str]]`
56 | async def test_contextmanager_with_decorator(val: int) -> AsyncIterator[str]: ...
57 | @asynccontextmanager
   |
info: Function defined here
   --> stdlib/contextlib.pyi:177:5
    |
175 |         ) -> bool | None: ...
176 |
177 | def asynccontextmanager(func: Callable[_P, AsyncIterator[_T_co]]) -> Callable[_P, _AsyncGeneratorContextManager[_T_co]]:
    |     ^^^^^^^^^^^^^^^^^^^ ---------------------------------------- Parameter declared here
178 |     """@asynccontextmanager decorator.
    |
info: rule `invalid-argument-type` is enabled by default

info[revealed-type]: Revealed type
  --> ty_test.py:61:13
   |
59 |     yield "hello world"
60 |
61 | reveal_type(test_contextmanager_with_decorator)
   |             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ `(val: str | int) -> _AsyncGeneratorContextManager[str, None]`
62 |
63 | async def call_asynccontextmanager_with_decorator():
   |

info[revealed-type]: Revealed type
  --> ty_test.py:65:21
   |
63 | async def call_asynccontextmanager_with_decorator():
64 |     async with test_contextmanager_with_decorator(123) as x:
65 |         reveal_type(x)
   |                     ^ `Unknown | str`
   |

Found 12 diagnostics
```

As you see the only variant thats actually working as expected is the one without the overloads.

The variant "using async context manager types" tells me the arguments to `@asynccontextmanager` are wrong cause of the overloads. Also the return type of `test_contextmanager_using_types` is set to `Unknown` instead of `str`.

With the last alpha version I was using the "wrong" typing version, which worked (but was pretty obviously wrong). Now it also complains about the `@asynccontextmanager` arguments plus has the wrong return type for the context manager (`CoroutineType[Any, Any, str | None]`, which even if you skip the coroutine part is still `str | None` instead of just `str`).

I then tried to using decorators in the overloads, as I thought this might just set the correct type, but again the arguments to `@asynccontextmanager` are wrong plus the return type of the context manager is `Unknown | str` instead of just `str`.

Is this still a restriction of the beta state of `ty` (btw. 🥳 for getting to beta, love the project) or am I doing something wrong?

**Note:** `mypy` does allow the version with (I think) correct typing, but the return value is still `Any`, so this also is not completely working there.

### Version

0.0.3

---

_Label `question` added by @ddanier on 2025-12-18 10:12_

---

_Label `calls` added by @sharkdp on 2025-12-18 13:09_

---

_Label `overloads` added by @sharkdp on 2025-12-18 13:09_

---

_Label `question` removed by @sharkdp on 2025-12-18 13:09_

---

_Comment by @sharkdp on 2025-12-18 13:10_

Thank you for reporting this.

This looks like a bug indeed (I did not have time to look into it further).

---

_Label `bug` added by @sharkdp on 2025-12-18 13:10_

---

_Comment by @ddanier on 2025-12-19 06:52_

Thanks for your feedback, this means I'm not doing something wrong, which is nice to know. Now waiting for a fix 😅

---

_Added to milestone `Stable` by @carljm on 2025-12-23 00:14_

---

_Comment by @carljm on 2026-01-08 19:18_

It looks like we are treating the overload implementation function as overloaded _before_ applying decorators to it, which is wrong -- we should first apply the decorators.

---

_Comment by @carljm on 2026-01-08 19:32_

Probably the same issue as #2278?

---

_Assigned to @dhruvmanila by @dhruvmanila on 2026-01-12 08:29_

---
