```yaml
number: 2420
title: "`object` type inferred when accessing possibly missing field with union type"
type: issue
state: closed
author: marcofrasvda
labels: []
assignees: []
created_at: 2026-01-09T17:36:24Z
updated_at: 2026-01-09T21:20:39Z
url: https://github.com/astral-sh/ty/issues/2420
synced_at: 2026-01-12T15:54:26Z
```

# `object` type inferred when accessing possibly missing field with union type

---

_@marcofrasvda_

### Summary

In the code below, the `uid` variable in the function is incorrectly inferred as `object` type when it should be `str | None`. The same can be reproduced in the global scope. I thought the problem was the usage of `hasattr()` with a ternary operator but it seems to work correctly with simpler types.

```py
class A:
    id: str

class B:
    # id: str - Uncomment this to see how type of "uid" changes below
    raw: bytes

class Container:
    payload: A | B

def process_data(container: Container):
    # This is inferred as `object` instead of `str | None`
    uid = container.payload.id if hasattr(container.payload, "id") else None
```

### Version

0.0.10

---

_Comment by @carljm on 2026-01-09 18:08_

Thanks for trying ty and taking the time to report an issue!

As it happens, ty's type inference is correct here. There could be a subclass of `B` that could have an `id` attribute of any type, so `hasattr(container.payload, "id")` does not guarantee that `container.payload` is of type `A`, or that `container.payload.id` is of type `str`.

If you want to tell ty that subclasses of `B` are not possible and should not be considered, you can decorate it with `typing.final` and get the desired type inference: https://play.ty.dev/30b16617-013b-47aa-9f45-57c4f5f44b38

---

_Closed by @carljm on 2026-01-09 18:08_

---

_Comment by @marcofrasvda on 2026-01-09 21:20_

Oh, that's understandable but surprising behavior, at least coming from Pyright. I did not know about `final` so I'll accept it's a skill issue until I get more comfortable with how ty works (and RTFM I suppose). Thank you for your kind explanation and example!

---
