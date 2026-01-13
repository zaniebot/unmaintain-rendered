```yaml
number: 2441
title: Avoid literal promotion for throwaway (non-escaping) invariant containers
type: issue
state: open
author: hamdanal
labels:
  - generics
assignees: []
created_at: 2026-01-11T09:21:14Z
updated_at: 2026-01-13T02:33:37Z
url: https://github.com/astral-sh/ty/issues/2441
synced_at: 2026-01-13T03:19:30Z
```

# Avoid literal promotion for throwaway (non-escaping) invariant containers

---

_@hamdanal_

#1240 suggests to stop adding `| Unknown` in places where ty currently adds it for better gradual typing.
https://github.com/astral-sh/ty/issues/1240#issuecomment-3729947824 is a recent great news in this regard as it proposes a very well thought solution - to stop adding the union with `Unknown` except for certain cases where the assigned value is a singleton like `None`. There are some places where even this special case isn't needed as the value used if final somewhat, either by being assigned to a `Final` target or by being a never-assigned throwaway expression. Here is a non-exhaustive list of these places:

```py
from typing import Final, reveal_type

# Final variable
_1: Final = reveal_type([1, 2, 3])

# Throwaway loop iterable
for _2 in reveal_type([1, 2, 3]): ...

# Throwaway comprehension loop iterable
_3 = [_ for _ in reveal_type([1, 2, 3])]

# Throwaway function argument
_4 = map(str, reveal_type([1, 2, 3]))
```

All these `reveal_type` currently reveal `list[Unknown | int]`. With the solution proposed in https://github.com/astral-sh/ty/issues/1240#issuecomment-3729947824 they will become `list[int]`. However, if we replace `[1, 2, 3]` by , say, `[None]` the `Unknown` union will still be used. I think that won't be necessary with no fallout as the value in these cases is kind of "final". I'd also argue that a more precise type could be inferred here, like `list[Literal[1, 2, 3]]` in the examples above (well, maybe, unless I am missing something).

---

_Comment by @AlexWaygood on 2026-01-11 09:34_

Hmm, but `Final` doesn't mean immutable — it just means that the variable can't be reassigned. I wouldn't want to emit false-positive diagnostics on things like this:

```py
X: Final = [None, None, None]

if foo:
    X[1] = Foo

```

Or this:

```py
X: Final = [None, None, None]

if foo:
    X.append(42)
```

I like the idea of inferring more precise types for throwaway loop variables. But I think you'd basically never iterate over a throwaway list that only contains `None`, so I don't think we'd be likely to do any unioning with `Unknown` for the vast majority of throwaway lists anyway once we've implemented https://github.com/astral-sh/ty/issues/1240#issuecomment-3729947824. So that sort-of feels like a separate issue to add a new heuristic where we avoid `Literal` promotion for the elements of "immediately consumed" invariant containers that aren't too large 

---

_Comment by @hamdanal on 2026-01-11 09:51_

> Hmm, but `Final` doesn't mean immutable — it just means that the variable can't be reassigned.

I was sure I missed something!

> I like the idea of inferring more precise types for throwaway loop variables. But I think you'd basically never iterate over a throwaway list that only contains `None`, so I don't think we'd be likely to do any unioning with `Unknown` for the vast majority of throwaway lists anyway once we've implemented [#1240 (comment)](https://github.com/astral-sh/ty/issues/1240#issuecomment-3729947824). So that sort-of feels like a separate issue to add a new heuristic where we avoid `Literal` promotion for the elements of "immediately consumed" invariant containers that aren't too large

Sounds reasonable

---

_Comment by @AlexWaygood on 2026-01-11 14:13_

The proposal to avoid `Literal` promotion in cases of "throwaway" list/dict/set literals like `for x in [1, 2, 3]` is similar to https://github.com/astral-sh/ty/issues/2280, where the request is for us to avoid `Literal` promotion for

```py
x = frozenset({1, 2, 3})
```

---

_Comment by @carljm on 2026-01-13 02:33_

Thanks for the suggestion! I'll remove the specific mention of `| Unknown` unioning (since that's covered by #1240, and what remains of it will be considered a form of "literal promotion") and retitle this to just be about avoiding literal promotion in throwaway cases where we can prove the container does not "escape".

---

_Renamed from "Safe places to totally stop unioning with `Unknown` and maybe infer more precise types" to "Avoid literal promotion for throwaway (non-escaping) invariant containers" by @carljm on 2026-01-13 02:33_

---

_Label `generics` added by @carljm on 2026-01-13 02:33_

---
