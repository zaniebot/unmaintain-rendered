```yaml
number: 2434
title: "`\"\".join(typed_as_str)` should be typed as `str`, not `LiteralString`"
type: issue
state: open
author: Salamandar
labels:
  - Protocols
assignees: []
created_at: 2026-01-10T10:02:56Z
updated_at: 2026-01-13T19:37:44Z
url: https://github.com/astral-sh/ty/issues/2434
synced_at: 2026-01-13T20:36:46Z
```

# `"".join(typed_as_str)` should be typed as `str`, not `LiteralString`

---

_@Salamandar_

### Summary

There are multiple issues:
* typeshed defines str.split() and str.join() with a return type of LiteralString or list[LiteralString]
  https://github.com/python/typeshed/issues/10887
* `ty` considers str as LiteralString:

```py
a = str()
reveal_type(a)
page_lines = "".join(some_method_returning_str()).split("\n")
reveal_type(page_lines)
```
gives `Literal[""]` and `list[LiteralString]`

while `mypy` gives:
```
note: Revealed type is "builtins.str"
note: Revealed type is "builtins.list[builtins.str]"
```

and as list is invariant, we can't pass `list[LiteralString]` to functions expecting a list[str]…

### Platform

Linux

### Version

0.0.8, 0.0.11

### Python version

3.13.11

---

_Label `bug` added by @Salamandar on 2026-01-10 10:02_

---

_Label `bug` removed by @AlexWaygood on 2026-01-10 11:17_

---

_Comment by @AlexWaygood on 2026-01-10 11:25_

Thanks for the report!

We intend to soon change our behaviour to mypy's to allow you to use an explicit inline annotation to do a "safe upcast". In this case, you want ty to infer a `str` rather than `Literal[""]` for your initial assignment, and once we implement this behaviour change, you'd be able to achieve that with

```py
a: str = str()
```

Apart from that, the other thing ty could do here would be to _implicitly_ infer (even without the annotation) that we need to infer the broader type here at the assignment of `a` to avoid false positives later on in the scope. That _might_ be possible with sufficiently advanced bidirectional inference, but it seems hard in this specific case.

---

_Comment by @AlexWaygood on 2026-01-12 08:13_

As a short-term measure you can use

```py
from typing import cast

a = cast(str, str())
```

But I realise that's not ideal.

---

_Label `bidirectional inference` added by @ibraheemdev on 2026-01-13 01:07_

---

_Comment by @carljm on 2026-01-13 02:01_

This is two (related) reports in one. In the OP example, the type of `a` is irrelevant to the type of `page_lines`.

In the case of `a`, I think ty's inference is fine; we do know that `a` is `Literal[""]` after `a = str()`, and it causes no harm to infer it as such. We should allow `a: str = str()` as an explicit upcast, and we will with #136. Other type checkers don't infer `str()` as `Literal[""]`, but they do infer `""` as `Literal[""]`; there's no practical difference.

The case of `page_lines` is more complex, because inserting an annotation `page_lines: list[str]` actually results in a type error, since we still infer the RHS as `list[LiteralString]`, which is not assignable to `list[str]`. Thus even #136 won't help here.

Breaking down the example:

`typed_as_literal_string.split("\n")` should be typed as `list[LiteralString]` according to the overloads of `str.split` in typeshed, and other type-checkers agree. Pyright and pyrefly also error on `x: list[str] = typed_as_literalstring.split("\n")`, just like we do. (Mypy doesn't support `LiteralString` at all, so its behavior is irrelevant here.) There may be a usability issue here, but if so it's shared across the typing ecosystem, and the solution is probably to just upcast `typed_as_literalstring` to `str` before calling `.split()` on it.

Which gets to the place where there might be a bug in ty. We infer `"".join(returns_str())` as `LiteralString`, but other type checkers infer it as `str`. And according to the overloads of `str.join` in typeshed, it looks like we are wrong. We are picking the overload

```py
    def join(self: LiteralString, iterable: Iterable[LiteralString], /) -> LiteralString:
```

The root cause of this seems to be that we wrongly think `str` is assignable to `Iterable[LiteralString]`. And the root cause of _that_ seems to be that we don't consider the annotated type of `self` when determining protocol assignability. `Iterable` is a protocol with just one method, `__iter__`, and `str` has the following overloads for `__iter__`:

```py
    @overload
    def __iter__(self: LiteralString) -> Iterator[LiteralString]:
        """Implement iter(self)."""

    @overload
    def __iter__(self) -> Iterator[str]: ...  # type: ignore[misc]
```

It seems like we consider the first overload here to make `str` a subtype of `Iterator[LiteralString]` -- but we shouldn't even consider that overload, due to the more specific annotated type of `self`.

---

_Renamed from "Strings are considered as LiteralStrings" to "`str` should not be assignable to `Iterable[LiteralString]`" by @carljm on 2026-01-13 02:02_

---

_Label `bidirectional inference` removed by @carljm on 2026-01-13 02:02_

---

_Label `Protocols` added by @carljm on 2026-01-13 02:02_

---

_Comment by @carljm on 2026-01-13 02:03_

I'm not sure how we'd apply bidirectional inference in this case, since `str.split("\n")` is not generic -- given the base type inferred as `LiteralString`, we are simply picking the right overload for `split()`.

---

_Added to milestone `Stable` by @carljm on 2026-01-13 02:03_

---

_Renamed from "`str` should not be assignable to `Iterable[LiteralString]`" to "`"".join(typed_as_str)` should be typed as `str`, not `LiteralString`" by @carljm on 2026-01-13 02:05_

---

_Comment by @AlexWaygood on 2026-01-13 08:03_

> I'm not sure how we'd apply bidirectional inference in this case, since `str.split("\n")` is not generic -- given the base type inferred as `LiteralString`, we are simply picking the right overload for `split()`.

The idea is that we could potentially use advanced whole-of-scope bidirectional inference to avoid inferring LiteralString in the first place, and instead we could infer str there because of the fact that inferring the more precise type causes type errors later on in the scope.

---

_Comment by @carljm on 2026-01-13 19:35_

> The idea is that we could potentially use advanced whole-of-scope bidirectional inference to avoid inferring LiteralString in the first place, and instead we could infer str there because of the fact that inferring the more precise type causes type errors later on in the scope.

I'm skeptical that this will be a workable approach. The "whole-scope bidirectional inference" we've discussed so far in #1473  (which is already pretty ambitious IMO) at least has a pretty clear idea of exactly where we need to look ahead for relevant type contexts ("uses of this particular container"). Making inference decisions based on a much fuzzier concept of "does this cause type errors later on" (where the errors might be due to several inference steps beyond the type we are currently inferring) is I think a road we should not go down.

In any case I think we should keep _this_ issue narrowly scoped to the wrong assignability of `str` to `Iterable[LiteralString]` causing us to pick the wrong overload of `str.join`, as outlined above. That's where we are going wrong relative to other type checkers in this case.

---
