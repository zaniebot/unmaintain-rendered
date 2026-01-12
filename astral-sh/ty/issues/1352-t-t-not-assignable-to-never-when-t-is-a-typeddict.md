```yaml
number: 1352
title: "T & ~T not assignable to Never when T is a `TypedDict`"
type: issue
state: closed
author: alecbarber
labels:
  - type properties
  - typeddict
assignees: []
created_at: 2025-10-14T13:20:01Z
updated_at: 2025-12-20T01:12:49Z
url: https://github.com/astral-sh/ty/issues/1352
synced_at: 2026-01-12T15:54:25Z
```

# T & ~T not assignable to Never when T is a `TypedDict`

---

_@alecbarber_

### Summary

Probably related to #154 (looks like `TypedDict` support is WIP)

`T & ~T` doesn't simplify to `Never` when `T` is a `TypedDict`. For example:

```python
from typing import TypeIs, assert_never, TypedDict


class Qux(TypedDict):
    pass

def is_qux(x: object) -> TypeIs[Qux]:
    raise NotImplementedError()

def foo(bar: Qux) -> None:
    if is_qux(bar):
        return None
    else:
        reveal_type(bar)  # revealed-type: Revealed type: `Qux & ~Qux`
        assert_never(bar)  # type-assertion-failure: Argument does not have asserted type `Never`
```

In this situation, I would expect `ty` to match `mypy` and infer that the `else:` branch is unreachable.

Playground link: https://play.ty.dev/7c4d3d6c-ebeb-4d85-ab21-f17c174fe206

### Version

ac2c53037

---

_Label `type properties` added by @sharkdp on 2025-10-14 13:59_

---

_Comment by @sharkdp on 2025-10-14 14:02_

Thank you for reporting this.

It's likely that this is indirectly caused by the lack of proper subtyping/assignability/equivalence relations on `TypedDict`s (the "Implement structural assignability" point in #154). We currently treat them as if they were sub/super-types of everything, so it's possible that this leads to the outcome above when combined with intersections and negation types.

We plan to implement that soon. I think it makes sense for this ticket to be left open to make sure it's really the root cause.

---

_Label `typeddict` added by @carljm on 2025-11-06 13:57_

---

_Comment by @carljm on 2025-12-20 01:12_

This is now fixed! (It was fixed by https://github.com/astral-sh/ruff/pull/22044)

---

_Closed by @carljm on 2025-12-20 01:12_

---
