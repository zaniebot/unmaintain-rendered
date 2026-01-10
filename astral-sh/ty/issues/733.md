```yaml
number: 733
title: "ty reports `urllib.parse.urlunparse` as returning `Literal[b\"\"]`"
type: issue
state: closed
author: charliermarsh
labels:
  - generics
  - Protocols
assignees: []
created_at: 2025-06-30T22:36:53Z
updated_at: 2025-10-08T18:37:32Z
url: https://github.com/astral-sh/ty/issues/733
synced_at: 2026-01-10T02:06:24Z
```

# ty reports `urllib.parse.urlunparse` as returning `Literal[b""]`

---

_Issue opened by @charliermarsh on 2025-06-30 22:36_

Given:

```python
import urllib.parse


def func(url: str) -> str:
    parsed = urllib.parse.urlparse(url)
    return urllib.parse.urlunparse(parsed)
```

Returns:

```
error[invalid-return-type]: Return type does not match returned value
 --> main.py:4:23
  |
4 | def func(url: str) -> str:
  |                       --- Expected `str` because of return type
5 |     parsed = urllib.parse.urlparse(url)
6 |     return urllib.parse.urlunparse(parsed)
  |            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ expected `str`, found `Literal[b""]`
  |
info: rule `invalid-return-type` is enabled by default

Found 1 diagnostic
```

---

_Comment by @carljm on 2025-07-01 00:25_

This is at least in part an issue of `NamedTuple` support (specifically, supporting generic `NamedTuple`). `urlparse` returns a `ParseResult`, which is a generic `NamedTuple` over `AnyStr`. [We understand](https://play.ty.dev/b5e779c5-b5bf-4e21-bef1-02814fcfef37) that we have a `ParsedResult`, and that it inherits `_ParsedResultBase[str]`, but we still think it's an iterable over `Unknown`. So this would be https://github.com/astral-sh/ty/issues/545

Even then, I think step 5 of the overload evaluation algorithm should cause us to infer `Any` as the return type of the `urlunparse` call (since `Iterable[Unknown]` could materialize to match either overload), but we end up picking the first overload instead. I suspect this is due to https://github.com/astral-sh/ty/issues/669

Seems useful to leave this open for now, so we actually verify that this case works correctly once we've implemented both of the above.

---

_Label `feature` added by @carljm on 2025-07-01 00:26_

---

_Label `generics` added by @carljm on 2025-07-01 00:26_

---

_Label `feature` removed by @carljm on 2025-07-01 00:26_

---

_Comment by @AlexWaygood on 2025-08-13 18:18_

> [We understand](https://play.ty.dev/b5e779c5-b5bf-4e21-bef1-02814fcfef37) that we have a `ParsedResult`, and that it inherits `_ParsedResultBase[str]`, but we still think it's an iterable over `Unknown`. So this would be [#545](https://github.com/astral-sh/ty/issues/545)

We now have a _very_ complete understanding of the MRO of `ParseResult`:

```py
import urllib.parse

# revealed: tuple[<class 'ParseResult'>, <class '_ParseResultBase[str]'>, <class 'tuple[str, str, str, str, str, str]'>, <class 'Sequence[str]'>, <class 'Reversible[str]'>, <class 'Collection[str]'>, <class 'Iterable[str]'>, <class 'Container[str]'>, typing.Protocol, <class '_NetlocResultMixinStr'>, <class '_NetlocResultMixinBase[str]'>, typing.Generic, <class '_ResultMixinStr'>, <class 'object'>]
reveal_type(urllib.parse.ParseResult.__mro__)
```

https://play.ty.dev/5403f5fe-1ed7-4ecc-8c7e-474f38d8f696

Well... some of the classes there look like they're in the wrong order in that MRO, but that shouldn't matter here :P (and I think probably has the same underlying cause as https://github.com/astral-sh/ty/issues/492).

Anyway, we're still incorrectly selecting the first overload here, which is still causing the false positive in the OP -- that's because even though we understand that `ParseResult` inherits from `Iterable[str]`, we still think that it's assignable to `Iterable[None]`. That should be fixed shortly when we improve our subtyping/assignability logic for protocols with method members to look at the signatures of the methods.

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-08-22 13:10_

---

_Label `Protocols` added by @AlexWaygood on 2025-08-22 13:10_

---

_Closed by @AlexWaygood on 2025-10-08 18:37_

---
