```yaml
number: 1459
title: Specialized typevars are wrongly considered still generic for variance inference
type: issue
state: open
author: MeGaGiGaGon
labels:
  - bug
  - generics
assignees: []
created_at: 2025-10-30T23:53:42Z
updated_at: 2025-10-31T18:28:44Z
url: https://github.com/astral-sh/ty/issues/1459
synced_at: 2026-01-12T15:54:25Z
```

# Specialized typevars are wrongly considered still generic for variance inference

---

_@MeGaGiGaGon_

### Summary

Hopefully I understand variance enough for this.

A type being given to `self` in a method should not affect the variance of a class generic. Even if there is some reason I don't know about why it should, then since both covariance and contravariance are affected bivariance should be too,

https://play.ty.dev/37f6360e-1d6d-42f7-9551-6f7cd441efae
```py
from ty_extensions import static_assert, is_subtype_of

class Bivariant[T]:
    def uses_int_self(self: Bivariant[int]):
        ...

static_assert(is_subtype_of(Bivariant[int], Bivariant[object]))
static_assert(is_subtype_of(Bivariant[object], Bivariant[int]))

class Covariant[T]:
    def make_cls_covariant(self) -> T:
        raise NotImplemented

    def uses_int_self(self: Covariant[int]):
        ...

# Should not raise?
static_assert(is_subtype_of(Covariant[int], Covariant[object]))

class Contravariant[T]:
    def make_cls_contravariant(self, value: T):
        raise NotImplemented

    def uses_int_self(self: Contravariant[int]):
        ...

# Should not raise?
static_assert(is_subtype_of(Contravariant[object], Contravariant[int]))
```

### Version

playground (3be3a10a2)

---

_Comment by @carljm on 2025-10-31 18:27_

Thanks for the report! Yes, I think you are correct. I don't think this is actually related to `self` annotations, either. This method should also not affect variance inference, but it currently does in ty:

```py
    def uses_other(self, other: Covariant[int]): ...
```

So I think the root cause is that typevars which are "specialized away" should be ignored in variance inference, but we currently don't ignore them. Our internal representation of `Covariant[int]` includes the typevar, but `Covariant[int]` is not a generic type: for variance inference purposes it should not be considered to include the typevar at all. (I haven't verified this with code exploration, it's just my intuition of what is probably wrong.)

---

_Label `bug` added by @carljm on 2025-10-31 18:27_

---

_Label `generics` added by @carljm on 2025-10-31 18:27_

---

_Renamed from "Adding a self type to co/contra variant class methods makes them invariant" to "Specialized typevars are wrongly considered still generic for variance inference" by @carljm on 2025-10-31 18:28_

---

_Added to milestone `GA` by @carljm on 2025-10-31 18:28_

---
