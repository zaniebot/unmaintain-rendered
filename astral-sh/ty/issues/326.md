```yaml
number: 326
title: Output type of sorted() does not get properly evaluated
type: issue
state: closed
author: skraeven
labels:
  - bug
  - generics
  - overloads
assignees: []
created_at: 2025-05-12T10:59:19Z
updated_at: 2025-05-13T08:08:19Z
url: https://github.com/astral-sh/ty/issues/326
synced_at: 2026-01-10T02:34:09Z
```

# Output type of sorted() does not get properly evaluated

---

_Issue opened by @skraeven on 2025-05-12 10:59_

### Summary

I'm getting an unexpected diagnostic when using sorted() in python 3.11

Minimal example:
```
def some_function() -> list[str]:
    some_list: list[str] = ["3", "1", "2"]
    return sorted(some_list)
```

Gives the following error:
```
  |
1 | def some_function() -> list[str]:
  |                        --------- Expected `list[str]` because of return type
2 |     some_list: list[str] = ["3", "1", "2"]
3 |     return sorted(some_list)
  |            ^^^^^^^^^^^^^^^^^ Expected `list[str]`, found `list[SupportsRichComparisonT]`
  |
info: `invalid-return-type` is enabled by default

Found 1 diagnostic
```

### Version

ty 0.0.0-alpha.8

---

_Label `bug` added by @sharkdp on 2025-05-12 11:05_

---

_Label `generics` added by @sharkdp on 2025-05-12 11:05_

---

_Comment by @dcreager on 2025-05-12 12:50_

`sorted` is an [overloaded definition](https://github.com/astral-sh/ruff/blob/797eb709044d0a9edf02f2a974e2a2fb13f499d8/crates/ty_vendored/vendor/typeshed/stdlib/builtins.pyi#L1768-L1773) in the typeshed:

```py
@overload
def sorted(
    iterable: Iterable[SupportsRichComparisonT], /, *, key: None = None, reverse: bool = False
) -> list[SupportsRichComparisonT]: ...
@overload
def sorted(iterable: Iterable[_T], /, *, key: Callable[[_T], SupportsRichComparison], reverse: bool = False) -> list[_T]: ...
```

So this has the same underlying cause as #314, and should be fixed by https://github.com/astral-sh/ruff/pull/18020

---

_Label `overloads` added by @AlexWaygood on 2025-05-12 13:13_

---

_Comment by @carljm on 2025-05-12 14:59_

It looks to me like https://github.com/astral-sh/ruff/pull/18020 is not sufficient to fix this; because the uses of `SupportsRichComparisonT` in the signatures of the overloads of `sorted` are nested (in one case inside `Iterable`, and in the other case inside `Callable`), fixing this will also require a more complete solver.

---

_Comment by @dcreager on 2025-05-12 17:49_

> It looks to me like [astral-sh/ruff#18020](https://github.com/astral-sh/ruff/pull/18020) is not sufficient to fix this; because the uses of `SupportsRichComparisonT` in the signatures of the overloads of `sorted` are nested (in one case inside `Iterable`, and in the other case inside `Callable`), fixing this will also require a more complete solver.

Ah right, good catch!  `Iterable` should be handled by https://github.com/astral-sh/ruff/pull/18021 (or at least, by the proposed follow-on PR to [support protocols too](https://github.com/astral-sh/ruff/pull/18021#discussion_r2083699336)).  `Callable` will be the tricky one, since it's not a simple 1:1 induction down both sides.

---

_Comment by @sharkdp on 2025-05-13 08:08_

Closing this, as the original bug is not reproducible anymore: https://play.ty.dev/860ce801-a754-469e-a3db-5db726185272

> `Callable` will be the tricky one, since it's not a simple 1:1 induction down both sides.

I guess this should be tracked somewhere else, but feel free to reopen this, if it's easier.

---

_Closed by @sharkdp on 2025-05-13 08:08_

---
