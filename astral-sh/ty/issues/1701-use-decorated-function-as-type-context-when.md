```yaml
number: 1701
title: Use decorated function as type context when inferring decorator expression (or more aggressively promote literals in decorators)
type: issue
state: open
author: patrick91
labels:
  - bug
  - generics
assignees: []
created_at: 2025-12-01T11:04:15Z
updated_at: 2025-12-02T02:35:09Z
url: https://github.com/astral-sh/ty/issues/1701
synced_at: 2026-01-10T01:58:59Z
```

# Use decorated function as type context when inferring decorator expression (or more aggressively promote literals in decorators)

---

_Issue opened by @patrick91 on 2025-12-01 11:04_

### Summary

Not sure if the title is descriptive enough, anyway, I have this decorator:

```python
from typing import Literal
from collections.abc import AsyncGenerator, Callable


def decorator[Item](
    message: Item,
) -> Callable[[Callable[[], AsyncGenerator[Item]]], Callable[[], AsyncGenerator[Item]]]:
    def inner(fn: Callable[[], AsyncGenerator[Item]]) -> Callable[[], AsyncGenerator[Item]]:
        return fn
    return inner
```

When I try to use and pass a message, the inferred type is too narrow, see:

```python
@decorator(message="hello")
async def my_generator() -> AsyncGenerator[Literal["hello"]]:
    yield "hello"

# this one fails with:
# Argument is incorrect: Expected `() -> AsyncGenerator[Literal["hello"], None]`, found `def my_generator2() -> AsyncGenerator[str, None]` (invalid-argument-type) [Ln 18, Col 1]
@decorator(message="hello")
async def my_generator2() -> AsyncGenerator[str]:
    yield "world"
```

I also tried with this, but I get the same output 😊

```python
message: str = "hello"

@decorator(message=message)
async def my_generator3() -> AsyncGenerator[str]:
    yield "world"
```

https://play.ty.dev/67bbea2d-a7d1-40a9-bce6-fa6787129244

### Version

ty 0.0.1-alpha.29 (0c3cae494 2025-11-28)

---

_Label `bug` added by @sharkdp on 2025-12-01 11:11_

---

_Label `generics` added by @sharkdp on 2025-12-01 11:11_

---

_Comment by @sharkdp on 2025-12-01 11:19_

Thank you for reporting this.

This will hopefully be fixed by #623.

> I also tried with this, but I get the same output 😊
> 
> ```py
> message: str = "hello"
> 
> @decorator(message=message)
> async def my_generator3() -> AsyncGenerator[str]:
>     yield "world"
> ```

That doesn't work because we still use `Literal["hello"]` as the inferred type of `message` in the local scope, even though it's declared as `str`. `@decorator(message=cast(str, "hello"))` would be a workaround.

---

_Comment by @carljm on 2025-12-02 02:23_

I don't think the new constraint solver alone will solve this. It will also require an additional feature (not otherwise discussed in an issue, AFAIK) to use the decorated function as part of a callable type context when inferring the decorator expression.

I'm not quite sure how to interpret mypy and pyright's behavior here. They seem to use this type context to some degree, because they are fine with both `my_generator` and `my_generator2`. If they were unconditionally doing literal promotion here with no type context, they would fail on `my_generator`. But they don't seem to fully use the type context, because they both fail on a `my_generator3` that returns `AsyncGenerator[object]`, even though it's also perfectly valid to infer `Item=object` from `@decorator(message="hello")`.

EDIT: Oh, actually mypy and pyright's behavior is fully explainable by literal promotion, without any use of type context, because `AsyncGenerator` is covariant in its yielded type.

So it seems like our choice here is to attempt to do better than mypy/pyright (but in a way that probably few users will care about) by using this type context, or to more aggressively promote literals in this case (which requires defining the limits of "this case") and match their behavior.

---

_Added to milestone `Stable` by @carljm on 2025-12-02 02:28_

---

_Renamed from "Generic type parameter inferred as literal type instead of widening to match decorated function's return type" to "Use decorated function as type context when inferring decorator expression" by @carljm on 2025-12-02 02:28_

---

_Renamed from "Use decorated function as type context when inferring decorator expression" to "Use decorated function as type context when inferring decorator expression (or more aggressively promote literals in decorators)" by @carljm on 2025-12-02 02:35_

---
