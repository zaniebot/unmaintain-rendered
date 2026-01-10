```yaml
number: 628
title: "Assignment narrowing doesn't work with descriptor"
type: issue
state: open
author: max-muoto
labels:
  - narrowing
  - attribute access
assignees: []
created_at: 2025-06-10T21:40:32Z
updated_at: 2025-07-23T22:53:14Z
url: https://github.com/astral-sh/ty/issues/628
synced_at: 2026-01-10T02:06:24Z
```

# Assignment narrowing doesn't work with descriptor

---

_Issue opened by @max-muoto on 2025-06-10 21:40_

### Summary

Take this simple example

```python
@dataclasses.dataclass
class Container:
    x: int = 3
    y: int | None = None


x = Container()
x.y = x.y or 34
reveal_type(x.y) # Revealed type: `int & ~AlwaysFalsy`
```

`ty` is able to infer that is both an integer and non-falsey. However, `y` is a descriptor this behavior no-longer works:

```python
from __future__ import annotations

from typing import Any, Self, overload, reveal_type, cast

import dataclasses


class Descriptor[T]:
    def __get__(self, obj: Any, owner: Any) -> T:
        return cast(T, 3)
    def __set__(self, obj: Any, value: T) -> None: ...


@dataclasses.dataclass
class Container:
    x: int = 3
    y: Descriptor[int | None] = Descriptor()


x = Container()
x.y = x.y or 34
reveal_type(x.y) # Revealed type is `int | None`, Pyright would infer `int`.
```

Ty playground: https://play.ty.dev/c6e6bb7e-afe0-44e0-8ef7-a2ed60b41e28



---

_Comment by @carljm on 2025-06-10 21:50_

This is intentional. The behavior of a descriptor is opaque; there’s no guarantee that the type assigned will be the same type observed on a later load. (In fact if a descriptor is used, that strongly suggests that something more complex might be going on internally.) So we cannot safely assume that just because we saw an int assigned, a later load will return an int. 

Id be interested to learn more about how pyright handles this to balance usability and correctness. 

---

_Label `narrowing` added by @carljm on 2025-06-10 21:51_

---

_Comment by @carljm on 2025-06-10 21:53_

Hmm, I guess in the specific case of your example, the use of a generic descriptor that receives and supplies the same type `T` could allow us to conclude that the type set must be the type later observed.

It [doesn't seem like pyright is making this distinction, though](https://pyright-play.net/?strict=true&code=MYGwhgzhAEAiBcAoaLoBMCmAzaB9XA5hgC74AUEGIWANNAJYB2ExYjwG80A9gEYBWGYMTrcA7owwAnLn0HCAlNAC0APgaNi0AD7QAct0lJUJ6FJIBXKY2gBGZKkw58lUrgpVaPAbIFCR0ABuYCAWnBpaugaSSmr6hpwOptAAdGmIiKCQMADCxo5csNAAvHBkChnAJdA55ZkpaNX25oEYIbjEAJ4ADhhkwA0KQA). [Seems more like](https://pyright-play.net/?strict=true&code=MYGwhgzhAEAiBcAoaLoBMCmAzaB9XA5hgC74AUEGIWANNAJYB2ExYjwG80A9gEYBWGYMTrcA7owwAnLn0HCAlNAC0APmgsZyVDqkkArlMbQARFm7cT2lJhz5KpXBSq0eA2QKEjoANzAh9TgZGYmgAH2gAOW5JJTUomM5rHWgAOnTERFBIGABhJB00LlhoAF44MgVM4DLoXMqs1LRagEZEPR8Mf1xiAE8ABwwyYCaFIA) it's just about whether the types received by `__set__` and returned by `__get__` are the same?

It's [easy to construct example descriptors](https://pyright-play.net/?strict=true&code=MYGwhgzhAEBqYgJYBMwBcD2AnAXAKGkOmQFMAzaAfUoHMS1qAKCEkMgGmkQDsI0xuwEjmgYARgCsSwNJwwB3biVyjJ0tAEpoAWgB8XbmmgAfaADkMS-ERvQs9AK5ZuBvgKEA6askQzqAbQAiPmwwOkCAXQIiUgpqFgZKZlYOVQkRcSkZTgA3BAdhAyNTCyUtPXNLYWjbNK9KHz9KIJCsMJJI6ABeaDyQAq4KPoHEGG4MI1KSaAFkXvzpgB5oAEYABmhWFkqlPDxQSBgASUF7AFsSQ2xrGPIqWnomFjZOHjdBQsz1OUVlDLUZOV9Dw0DdavY0E4XG9%2BB96o1Ei1MG1wlEbLF7gknik5JJ-llZPN%2BoUQUCdtVaoRMvDfIjgsj2p0esNpgBqVZ7fbgKDQADCYOgIBIUEoaAAFgJKOsRPAkKhkd04AgUOhsIwNDUDMBzpc0CRkCITtqSBcrlhFUadWb1ZzgIreTbgB4hSLxZL1oqVis8PYciQEKKAJ4ABxIjCdLogool3Clay00AAxOSZkYsA5DIgLpxg4GsIgaGKjBAwIGYAADAAyiD1bRA-i9EXL%2Bw8PGNpr1cx63t9-pAQdD4dbpxNuv1CeTACZU3YM2gsyQc3mC0XoCWy9AqzXlAgG02gA) where pyright's narrowing is not sound, but perhaps such descriptors aren't common in practice.

---

_Label `attribute access` added by @carljm on 2025-06-10 22:06_

---

_Comment by @erictraut on 2025-06-10 22:44_

> Id be interested to learn more about how pyright handles this to balance usability and correctness.

Pyright's behavior (which has been the same for the past four+ years) came out of a couple of discussions with the mypy maintainers. See [here](https://github.com/microsoft/pyright/issues/1814) for a discussion about descriptors and [here](https://github.com/python/mypy/issues/3004) specifically focusing on property objects. In both cases, we collectively concluded that the best way for type checkers to balance usability and correctness is to treat symmetric descriptors as "safe to narrow". As you point out, it's possible to construct examples where this heuristic breaks, but in practice it's almost always safe. I suppose you could make it a configuration option for users who want really strict rules.

In the case of asymmetric descriptors and properties, we agreed that it's best for type checkers not to apply narrowing. I was hoping to codify this guidance in the typing spec, but we haven't gotten around to it yet. (It's the last bullet item under "Methods & Attributes" on [this slide](https://docs.google.com/presentation/d/1ash1ellodiV6FTyVnY-Q6V2G5vJ68ycWBr6MYkayC1U/edit?slide=id.g2770f948faa_0_5#slide=id.g2770f948faa_0_5) that I presented last year.)

Interestingly, TypeScript always applies narrowing even for asymmetric accessors (which are the moral equivalent of descriptor objects in TypeScript). I guess that's not so surprising given that the TypeScript authors tend to favor usability over correctness when the two are at odds.

---

_Comment by @carljm on 2025-06-10 22:55_

Thanks for the context!

---

_Added to milestone `GA` by @carljm on 2025-07-23 22:53_

---
