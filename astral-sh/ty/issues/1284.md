```yaml
number: 1284
title: Should literal promotion recurse into generic types?
type: issue
state: closed
author: sharkdp
labels:
  - bug
assignees: []
created_at: 2025-09-30T14:12:08Z
updated_at: 2025-10-31T16:48:35Z
url: https://github.com/astral-sh/ty/issues/1284
synced_at: 2026-01-10T02:06:25Z
```

# Should literal promotion recurse into generic types?

---

_Issue opened by @sharkdp on 2025-09-30 14:12_

Literal promotion currently recurses down into types to also promote nested types. When doing type inference for generic container literals, this makes sense for unions:
```py
x = 1 if flag else "a"
xs = [x]  # list[Unknown | int | str]
```

But it seems wrong to descend into generic types (and possibly other `Type` variants)? For example:
```py
class Invariant[T]:
    x: T

    def __init__(self, value: T):
        self.x = value

def _(a: Invariant[Literal[1]]):
    xs = [a]  # list[Unknown | Invariant[int]]
```

Shouldn't we infer `list[Unknown | Invariant[Literal[1]]]` here?

---

_Label `bidirectional inference` added by @sharkdp on 2025-09-30 14:12_

---

_Comment by @AlexWaygood on 2025-09-30 14:19_

Yeah, it's unsound to promote invariant generics like that. It's only sound to promote a subtype to its supertype, but `Invariant[Literal[1]]` is not a subtype of `Invariant[int]`. This is definitely a bug.

---

_Label `bug` added by @AlexWaygood on 2025-09-30 14:19_

---

_Comment by @ibraheemdev on 2025-09-30 16:02_

I thought this would be an issue with generic constructors as well, but it seems like we ensure the argument is assignable to the promoted type:
```py
from typing import Literal

class Invariant[T]:
    x: T

    def __init__(self, value: T):
        self.x = value

def _(a: Invariant[Literal[1]]):
    x = Invariant(a) # error: [invalid-argument-type] Expected `Invariant[int]`, found `Invariant[Literal[1]]` 
```

Also note that because we eagerly promote generic constructors, it's not possible to create an instance of `Invariant[Literal[1]]` except with type-casting (not that this makes the recursive promotion any more sound).

---

_Comment by @sharkdp on 2025-09-30 17:08_

> Also note that because we eagerly promote generic constructors, it's not possible to create an instance of `Invariant[Literal[1]]` except with type-casting (not that this makes the recursive promotion any more sound).

Right, my example looked at lot easier in the beginning, until I noticed that as well 😄. I still wanted to open this discussion, because we (I) might want to use your literal promotion operation for other things as well.

---

_Comment by @ibraheemdev on 2025-10-14 21:17_

I wonder if the literal promotion should recurse at all. If we eagerly promote literals for generic constructors, collection types, and tuples*, we should only need to promote once at the top-level of each of those expression sites. I don't think there are really any other cases where we need to perform literal promotion.

*We don't currently perform literal promotion for tuples, so this would be a major change, but it seems more consistent overall.

---

_Comment by @AlexWaygood on 2025-10-14 21:43_

> We don't currently perform literal promotion for tuples, so this would be a major change

This would break quite a lot of things, unfortunately! If we infer `(3, 7)` as `tuple[int, int]` rather than `tuple[Literal[3], Literal[7]]`, we will infer `sys.version_info >= (3, 7)` as `bool` rather than a `Literal` type, regardless of what Python version the user has configured. That in turn will cause us to start treating all definitions inside `sys.version_info` branches in typeshed (there are _many_) as being possibly unbound regardless of the Python version the user has configured

---

_Comment by @AlexWaygood on 2025-10-14 21:57_

In general, as I think I've mentioned before, I'm not sure we should be doing Literal promotion at all in covariant or bivariant contexts, unless the user has explicitly requested Literal promotion using the declared type annotation. "Overly precise" Literal types only really cause ergonomic issues for invariant generics, and maybe contravariant generics, because it's easy to upcast a covariant or bivariant generic type to a less precise supertype. So I'd say what we currently do for `tuple` is desirable, and that we should try to do similar things for other covariant containers such as `frozenset`

---

_Assigned to @ibraheemdev by @ibraheemdev on 2025-10-17 14:39_

---

_Added to milestone `Beta` by @ibraheemdev on 2025-10-17 14:40_

---

_Label `bidirectional inference` removed by @ibraheemdev on 2025-10-24 20:16_

---

_Closed by @sharkdp on 2025-10-31 16:48_

---
