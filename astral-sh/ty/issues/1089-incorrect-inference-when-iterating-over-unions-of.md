```yaml
number: 1089
title: Incorrect inference when iterating over unions of types
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - Protocols
assignees: []
created_at: 2025-08-22T12:34:28Z
updated_at: 2025-10-08T18:37:33Z
url: https://github.com/astral-sh/ty/issues/1089
synced_at: 2026-01-10T02:06:24Z
```

# Incorrect inference when iterating over unions of types

---

_Issue opened by @AlexWaygood on 2025-08-22 12:34_

### Summary


I first thought this was a bug in `Type::try_iterate_with_mode()`, then I thought it might be a bug in generics solving, but finally I realised that this was a bug in protocol assignability/subtyping -- we currently think that `Iterator[str]` is a subtype of `Iterator[int]`, meaning we eagerly simplify the union of the two types together, leading us to infer the wrong type when iterating over a union of iterables if each element in the union has an `__iter__` method that returns `Iterator[T]`. And the vast majority of real-world `__iter__` methods are annotated as returning `Iterator[T]`. That means that we infer the correct type when iterating over `b` in the function below, but an incorrect type when iterating over `a`.

So this is a bug I was already aware of, but it had a consequence I wasn't aware of, leading me to spend some time debugging it just now. Hence why I'm writing it up. It also probably serves as a good test case for the future.

```py
from typing import Literal, reveal_type, Iterator
from ty_extensions import is_subtype_of

class StringIterator:
    def __next__(self) -> str:
        return "foo"

class IntIterator:
    def __next__(self) -> int:
        return 42

class StringIterable1:
    def __iter__(self) -> Iterator[str]:
        raise NotImplementedError

class IntIterable1:
    def __iter__(self) -> Iterator[int]:
        raise NotImplementedError

class StringIterable2:
    def __iter__(self) -> StringIterator:
        raise NotImplementedError

class IntIterable2:
    def __iter__(self) -> IntIterator:
        raise NotImplementedError

def f(a: StringIterable1 | IntIterable1, b: StringIterable2 | IntIterable2, c: Iterator[int] | Iterator[str]):
    for x in a:
        reveal_type(x)  # str -- should be `str | int`!

    reveal_type(a.__iter__())  # Iterator[str] -- should be `Iterator[str | int]`!
    reveal_type(c)  # Iterator[int]  -- should be `Iterator[int] | Iterator[str]` (or `Iterator[int | str]`)
    reveal_type(is_subtype_of(Iterator[int], Iterator[str]))  # `Literal[True]`, but should be `Literal[False]`

    for y in b:
        reveal_type(y)  # str | int (correct!)
```

https://play.ty.dev/86abc512-4b5f-4675-8689-9f36b92b163d

You can also reproduce this by iterating over unions of literals, unions of tuples, etc.

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2025-08-22 12:34_

---

_Label `Protocols` added by @AlexWaygood on 2025-08-22 12:34_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-08-22 12:34_

---

_Closed by @AlexWaygood on 2025-10-08 18:37_

---
