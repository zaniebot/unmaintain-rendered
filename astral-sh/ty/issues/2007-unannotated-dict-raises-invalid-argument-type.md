```yaml
number: 2007
title: "Unannotated dict raises `invalid-argument-type`"
type: issue
state: closed
author: jeertmans
labels:
  - needs-design
  - generics
assignees: []
created_at: 2025-12-17T12:52:12Z
updated_at: 2025-12-23T20:42:19Z
url: https://github.com/astral-sh/ty/issues/2007
synced_at: 2026-01-12T15:54:26Z
```

# Unannotated dict raises `invalid-argument-type`

---

_@jeertmans_

### Summary

Hi!

Not sure if this is a bug or a feature (https://github.com/astral-sh/ruff/pull/20927 ?), but an `invalid-argument-type` error is raise for the `**option` argument inside `baz` when it shouldn't (in my opinion).

Probably related to #1248.

A possible workaround is to declare an annotated temporary variable, like inside the `bar` function, but I guess the `baz` function should also be correct (especially as everything is encapsulated inside the function call, such that the inferred type of `options` doesn't depend input arguments).

Thank you very much for your help!

```python
from collections.abc import Mapping, Sequence
from typing import Any


def foo(x: int, option_a: Sequence[str] | None = None, option_b: Mapping[str, Any] | None = None, **other_options: Any) -> int:
    return x

def bar(x: int, options: Mapping[str, Any] | None = None) -> int:
    default: dict[str, Any] = {"option_a": ["a", "b"]}
    options = {**default, **(options or {})}
    return foo(x, **options)

def baz(x: int, options: Mapping[str, Any] | None = None) -> int:
    options = {"option_a": ["a", "b"], **(options or {})}
    return foo(x, **options)
```

Link to [playground](https://play.ty.dev/4e105c77-23a6-4c1e-8f63-be1ee13a502b).

### Version

ty 0.0.2

---

_Comment by @carljm on 2025-12-18 00:18_

Interesting, thanks for the report! This is closely related to #1248, but appears to be slightly different in terms of how it's handled by other type checkers. They seem to all infer `dict[str, Any]` for `options` in both cases, whereas we preserve more information in the second case about the actual value type we saw, which then causes the error. I'm not totally sure what the heuristic is that results in the `dict[str, Any]` type in the `baz` case. The only obvious "source" for `Any` is the type of the original `options` (`Mapping[str, Any]`), but it's not clear why that should erase the type information from the new `"option_a"` key.

---

_Label `needs-design` added by @carljm on 2025-12-18 00:18_

---

_Label `generics` added by @carljm on 2025-12-18 00:18_

---

_Added to milestone `Stable` by @carljm on 2025-12-18 00:18_

---

_Comment by @jeertmans on 2025-12-19 08:47_

Thanks for your reply @carljm!
> This is closely related to https://github.com/astral-sh/ty/issues/1248, but appears to be slightly different in terms of how it's handled by other type checkers

Yes, this is why I felt it could be interesting to report in another issue :-)

Maybe this issue is that `{"option_a": ["a", "b"]}` is typed (by `ty`) `dict[Unknown | str, Unknown | list[Unknown]]`, when it could actually be narrowed down to `dict[str, list[str]]`?

However, typing `default: dict[str, list[str]] = {"option_a": ["a", "b"]}` in `bar` makes the type check fail, raising:
```
Argument to function `foo` is incorrect: Expected `Mapping[str, Any] | None`, found `Unknown | list[str]` (invalid-argument-type)
```

Shouldn't `T | Any` (or `T | Unknown`) be compatible with `Any`?


---

_Comment by @carljm on 2025-12-23 00:09_

> Shouldn't `T | Any` (or `T | Unknown`) be compatible with `Any`?

The `Any` in the type of the `option_b` parameter is inside `Mapping[str, Any]`, so it doesn't really come into play. It is correct that `Unknown | list[str]` is not assignable to `Mapping[str, Any] | None`.

> typing `default: dict[str, list[str]] = {"option_a": ["a", "b"]}` in `bar` makes the type check fail

Yes, this is what I'd expect; the Unknowns aren't causing any problem. The only reason other type checkers are OK with this is because they erase more type information in the creation of `options` variable than we do. We preserve more of the known type of `default`; they throw away that information in favor of just `Any`. So giving a more precise type to `default` doesn't help us; only giving it a _less_ precise type, with key value of just `Any`, (like you do in `bar` above) helps.

---

_Comment by @jeertmans on 2025-12-23 08:02_

I see! Do you think this kind of issue can still be addressed by ty, or if on the user's side to explicitly provide type information?

---

_Comment by @carljm on 2025-12-23 20:18_

I think we should probably change ty to improve compatibility here.

After looking at this again, I think the heuristic other type checkers are using here is the one already discussed in #1248 -- if a dict literal would be inferred with a union value type, prefer just falling back to Any instead. So I think we can consider this a duplicate of #1248. Thanks again for reporting!

---

_Closed by @carljm on 2025-12-23 20:42_

---
