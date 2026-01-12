```yaml
number: 493
title: Tuples of unions, Unions of tuples
type: issue
state: open
author: sharkdp
labels:
  - type properties
assignees: []
created_at: 2025-05-23T06:48:21Z
updated_at: 2025-10-06T08:19:12Z
url: https://github.com/astral-sh/ty/issues/493
synced_at: 2026-01-12T15:54:23Z
```

# Tuples of unions, Unions of tuples

---

_@sharkdp_

An example that I got from [this post](https://discuss.python.org/t/specify-that-assert-type-checks-for-equivalence/92967). As mentioned there, it's probably hard to support this in full generality, but it seems like those base cases should maybe be handled:

```py
from ty_extensions import static_assert, is_equivalent_to, is_assignable_to, is_subtype_of

static_assert(is_equivalent_to(tuple[int | str], tuple[int] | tuple[str]))  # currently fails
static_assert(is_assignable_to(tuple[int | str], tuple[int] | tuple[str]))  # currently fails
static_assert(is_subtype_of(tuple[int | str], tuple[int] | tuple[str]))  # currently fails
```
https://play.ty.dev/fa049103-bd97-454c-891e-92bda33973a5

---

_Label `type properties` added by @sharkdp on 2025-05-23 06:48_

---

_Comment by @dcreager on 2025-05-23 14:18_

To make sure I'm following, this would only be valid for a one-element tuple, right? For multi-element tuples the equivalence wouldn't hold, since "tuple of union" is heterogeneous while "union of tuple" is homogeneous.

---

_Comment by @AlexWaygood on 2025-05-23 14:23_

@dcreager nope — heterogeneous tuples of any length that contain unions can be exploded into unions, according to the spec: https://typing.python.org/en/latest/spec/tuples.html#type-compatibility-rules

---

_Comment by @dcreager on 2025-05-23 15:10_

> nope — heterogeneous tuples of any length that contain unions can be exploded into unions, according to the spec: https://typing.python.org/en/latest/spec/tuples.html#type-compatibility-rules

Right, but if I'm reading that section correctly, the resulting expansion has to retain the heterogeneity:

> If multiple elements are union types, full expansion must consider all combinations. For example, `tuple[int | str, int | str]` is equivalent to `tuple[int, int] | tuple[int, str] | tuple[str, int] | tuple[str, str]`. Unbounded tuples cannot be expanded in this manner.

So the following does not hold:

```py
static_assert(is_equivalent_to(tuple[int | str, int | str], tuple[int | int] | tuple[str | str]))
```

---

_Comment by @sharkdp on 2025-05-23 20:44_

Another way to think about it: Tuples are products, unions are sums, and this is the distributive law:

```
(int + str) × (int + str) = int × int + int × str + str × int + str × str
```

"Full" explosion is not the only alternative form. The following two representations are also equivalent:

```
(int + str) × int + (int + str) × str
int × (int + str) + str × (int + str)
```

---

_Comment by @JelleZijlstra on 2025-05-23 21:16_

There was also a similar post here: https://discuss.python.org/t/unbounded-tuple-unions/92472, showing that these types are equivalent: `tuple[int, ...]` and `tuple[()] | tuple[int, *tuple[int, ...]]`. (I believe ty doesn't yet support such unpacked types.)

There's a lot of other variations to come up with; for example, these are equivalent:

```python
# Group 1
tuple[bool, ...] | tuple[object]
tuple[()] | tuple[object] | tuple[bool, bool, *tuple[bool, ...]]

# Group 2
tuple[Literal[False], ...] | tuple[Literal[True], ...] | tuple[bool, bool, *tuple[bool, ...]]
tuple[bool, ...]
```

I think ideally type checkers should detect the equivalence, but I'm not sure there's a general algorithm that will run in a reasonable time, and most of these are unlikely to come up in practice, so probably not worth worrying about too much.



---

_Comment by @sharkdp on 2025-06-10 09:39_

An example I got from reviewing https://github.com/astral-sh/ruff/pull/17643, which may or may not be related to this issue. I find it strange that we infer `Unknown` here, and not some `@Todo` type?


```py
from typing import reveal_type


def _(union_of_tuples: tuple[int] | tuple[None]):
    reveal_type(union_of_tuples[0])  # Unknown
```

---

_Comment by @carljm on 2025-06-10 16:42_

> An example I got

I think this is a distinct issue; I opened https://github.com/astral-sh/ty/issues/625

---

_Added to milestone `GA` by @carljm on 2025-07-23 23:10_

---
