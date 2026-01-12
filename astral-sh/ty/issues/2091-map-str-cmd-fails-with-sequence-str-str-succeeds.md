```yaml
number: 2091
title: "`map(str, cmd)` fails with `Sequence[str] & ~str`, succeeds with `Sequence[str]`"
type: issue
state: open
author: carljm
labels:
  - generics
  - set-theoretic types
assignees: []
created_at: 2025-12-18T22:02:17Z
updated_at: 2026-01-08T18:00:52Z
url: https://github.com/astral-sh/ty/issues/2091
synced_at: 2026-01-12T15:54:26Z
```

# `map(str, cmd)` fails with `Sequence[str] & ~str`, succeeds with `Sequence[str]`

---

_@carljm_

From #2087:

```py
from typing import Sequence, reveal_type

def test(command: Sequence[str] | str) -> str:
    reveal_type(command)  # Sequence[str]
    parts = map(str, command)  # works
    if isinstance(command, str):
        return command
    else:
        reveal_type(command)  # Sequence[str] & ~str
        parts = map(str, command)  # fails with "not assignable to `Iterable[Buffer]`"
        return ' '.join(parts)

print(test('echo hello'))
print(test(['echo', 'hello']))
```

The first `map` works, the second one fails; the only difference is the intersection with `~str`. It should not be possible for that to cause the assignment to fail, if the other intersection component succeeds.

---

_Added to milestone `Stable` by @carljm on 2025-12-18 22:02_

---

_Label `set-theoretic types` added by @carljm on 2025-12-18 22:02_

---

_Label `Protocols` added by @carljm on 2025-12-18 22:02_

---

_Comment by @AlexWaygood on 2025-12-18 22:12_

If `T`, `U` and `S` are all fully static, `T & ~U` is only assignable to `S` if `T` is a subtype of `S` and `U` is disjoint from `S`.

I think what's happening here is `Sequence[str]` is a subtype of `Iterable[Buffer]` but `str` is not disjoint from `Iterable[Buffer]`, meaning that `Sequence[str] & ~str` is not a subtype of `Iterable[Buffer]`, because all three types are fully static.

I'm not sure this has anything to do with protocols.

---

_Comment by @karlicoss on 2025-12-18 22:25_

https://play.ty.dev/6c580aa1-8aec-4e21-8950-b7ec53abd987

```py
from typing import Sequence, reveal_type

def test(command: Sequence[str] | int) -> str:
    reveal_type(command)  # Sequence[str] | int
    if isinstance(command, int):
        return str(command)
    else:
        reveal_type(command)  # Sequence[str] & ~int
        parts = map(str, command)  # fails with "not assignable to `Iterable[Buffer]`"
    return ' '.join(parts)

print(test(123))
print(test(['echo', 'hello']))
```

it still fails when the type is `Sequence[str] | int`, and `int` is definitely disjoint from `Sequence[str]`? 

---

_Comment by @AlexWaygood on 2025-12-18 22:40_

Hmm interesting. I can dig in more tomorrow if Carl doesn't beat me to it while I'm asleep :-)

---

_Comment by @carljm on 2025-12-18 22:41_

> If `T`, `U` and `S` are all fully static, `T & ~U` is only assignable to `S` if `T` is a subtype of `S` and `U` is disjoint from `S`

How do you reach this conclusion? I don't think that's right. In general, `T & U` is assignable to `S` if `T` is assignable to `S` or if `U` is assignable to `S`. This is the nature of an intersection: `T & U` is always a smaller type than either `T` or `U` alone. So if `T` is assignable to `S`, it does not matter at all what `U` is (whether it's a negation type or not). (It sounds like you might be describing the logic for `T | ~U` rather than for `T & ~U`.)

---

_Comment by @carljm on 2025-12-18 22:49_

> I'm not sure this has anything to do with protocols.

Yes, it's definitely possible that it doesn't.

One thing I observed is that `str` is not assignable to `Buffer`, so we shouldn't expect `Sequence[str]` to be assignable to `Iterable[Buffer]` in the first place. Which suggests that with the intersection we are maybe resolving the wrong overload for `map.__new__`?

This might be about overload resolution and/or bidirectional checking, rather than protocols?

---

_Renamed from "issue with intersections and Protocol assignability" to "`map(str, cmd)` fails with `Sequence[str] & ~str`, succeeds with `Sequence[str]`" by @carljm on 2025-12-18 22:53_

---

_Label `Protocols` removed by @carljm on 2025-12-18 22:53_

---

_Comment by @AlexWaygood on 2025-12-19 11:44_

> > If `T`, `U` and `S` are all fully static, `T & ~U` is only assignable to `S` if `T` is a subtype of `S` and `U` is disjoint from `S`
> 
> How do you reach this conclusion? I don't think that's right. In general, `T & U` is assignable to `S` if `T` is assignable to `S` or if `U` is assignable to `S`. This is the nature of an intersection: `T & U` is always a smaller type than either `T` or `U` alone. So if `T` is assignable to `S`, it does not matter at all what `U` is (whether it's a negation type or not). (It sounds like you might be describing the logic for `T | ~U` rather than for `T & ~U`.)

Right, sorry, I got that completely backwards. I was reading the first of [these two branches](https://github.com/astral-sh/ruff/blob/e177cc2a5a5d5e25c06ea6b0bbac9209f37fe756/crates/ty_python_semantic/src/types.rs#L2428-L2497), when I should have been reading the second one.

It still seems _unlikely_ to me that this is protocol-related, given that our `Type::Intersection` branches are higher than our `Type::ProtocolInstance()` branch in `Type::has_relation_to_impl`. But it's possible...

---

_Removed from milestone `Stable` by @carljm on 2026-01-05 17:46_

---

_Added to milestone `Pre-stable 1` by @carljm on 2026-01-05 17:46_

---

_Label `generics` added by @carljm on 2026-01-08 17:59_

---

_Comment by @carljm on 2026-01-08 18:00_

This may be related to #1880 ?

---
