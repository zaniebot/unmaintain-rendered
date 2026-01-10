---
number: 2074
title: Annotated Self changes variance of class
type: issue
state: open
author: Matt-Ord
labels:
  - generics
  - typing semantics
assignees: []
created_at: 2025-12-18T14:42:21Z
updated_at: 2026-01-08T19:20:33Z
url: https://github.com/astral-sh/ty/issues/2074
synced_at: 2026-01-10T01:51:14Z
---

# Annotated Self changes variance of class

---

_Issue opened by @Matt-Ord on 2025-12-18 14:42_

### Summary

```python
class A[M: BasisMetadata = BasisMetadata]:
    def __init__(self, metadata: M) -> None:
        self._metadata = metadata

    def makes_it_covariant(self) -> M:
        """An example method that makes the object covariant."""
        return self._metadata

    def makes_it_invariant(self: A[Any]) -> None:
        """This (incorrectly?) makes the object invariant."""
```

I've use this along with a similar pattern like
```py
    def makes_it_invariant[M1: BasisMetadata = BasisMetadata](
        self: A[M1], other: M1
    ) -> Never:
        """This (incorrectly?) makes the object invariant."""
```

### Version

0.0.2

---

_Comment by @AlexWaygood on 2025-12-18 14:50_

Here's a [pyright playground](https://pyright-play.net/?pyrightVersion=1.1.405&pythonVersion=3.13&reportUnreachable=true&code=GYJw9gtgBA%2BjwFcAuCQFM5QJYQA5hCSgEMA7UsJYpLMUgZwChRIokBPXLUgc2zwJEAgqXaNGAYwA2xevSgAhWVnoBZNFQAm1YgC4oAOiOSZcqAGUEAI3VadACiX0Vt4tqoBKfUYPjps%2BSEAbVV9JxcNNx0AXV1GKASoTTRgWBhuLCQ4e3o0KWAAGigISPc9KFUPKABaAD4oADk6NDjEtqhc-IMYErsqKABeYtKdcTbk1IhiAGs0enSsiTAAN2IQLDIkHLzgKrqK1vaEgCJTkSg0AA9iPCk0YaQACzBNNkfqYpm5t-uwKwArNASIhLVbrTYGU7HeJHKDoFAgUgdHbdXpRKhjRITT6zeaZdKkMEbUhbTrAfTBETsaJ7epNUgtGFHKEAFUeKig9m4SxA6GBUnYAH4qlNcT8oH9AcDsIS1sSkJDTuJscB7JcKUFwmoRlQad5jIxsTw1RrLDYdcQ9UyoKrLh5GEA) showing that pyright infers this class as covariant, whereas we [do not](https://play.ty.dev/283289c4-acee-4763-8f30-e81c3d650de3). I wondered if pyright's behaviour might be because the `self` parameter is annotated with `self: A[Any]` rather than `self: A[M]`, but pyright has the same behaviour if you change the parameter annotation to `self: A[M]`. It looks like it is applying special handling due to the fact that it is the `self` parameter specifically.

---

_Label `typing semantics` added by @AlexWaygood on 2025-12-18 14:50_

---

_Comment by @carljm on 2025-12-23 00:31_

I think there are actually two bugs here:

1. It seems like we wrongly consider typevars that are in the generic context of an annotation but specialized away, as "used" in the annotation for purposes of variance inference. If this were fixed, then `A[Any]` (or `A[WhateverConcreteType]`) in any method parameter annotation should not cause invariance of `M`, but `A[M]` should.
2. `self` is different; type checkers ignore it for variance purposes because it is not provided explicitly. (Well, there's some subtlety here around use of unbound methods on `type[...]` types, but `type[...]` types are pretty unsound anyway...) It looks like maybe we don't do that, either, since `self: A[M]` doesn't make `A` invariant in `M` for other type checkers, but does for us.

---

_Added to milestone `Stable` by @carljm on 2025-12-23 00:31_

---

_Label `generics` added by @carljm on 2025-12-23 00:31_

---

_Comment by @carljm on 2026-01-08 19:20_

Point 1 is #1459

---
