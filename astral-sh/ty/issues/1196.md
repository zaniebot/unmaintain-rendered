```yaml
number: 1196
title: "Argument type `Top[list[Unknown]]` does not satisfy upper bound `Bottom[MutableSequence[Unknown]]` of type variable `Self`"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - generics
assignees: []
created_at: 2025-09-17T08:26:47Z
updated_at: 2025-09-23T12:02:27Z
url: https://github.com/astral-sh/ty/issues/1196
synced_at: 2026-01-10T02:06:25Z
```

# Argument type `Top[list[Unknown]]` does not satisfy upper bound `Bottom[MutableSequence[Unknown]]` of type variable `Self`

---

_Issue opened by @sharkdp on 2025-09-17 08:26_

### Summary

On https://github.com/astral-sh/ruff/pull/18007, we see the following problem:

```py
def f(obj: object):
    if isinstance(obj, list):
        obj.clear()
```

```
/home/shark/playground/test.py:8:9: error[invalid-argument-type] Argument to bound method `clear` is incorrect: Argument type `Top[list[Unknown]]` does not satisfy upper bound `Bottom[MutableSequence[Unknown]]` of type variable `Self`
/home/shark/playground/test.py:8:9: error[invalid-argument-type] Argument to bound method `clear` is incorrect: Expected `Self@clear`, found `Top[list[Unknown]]`
```

It looks like the upper bound is incorrect (`Bottom[MutableSequence[Unknown]]` instead of `Top[MutableSequence[Unknown]]`.

### Version

https://github.com/astral-sh/ruff/pull/18007 (ed798a35ee8378d18c014a3c8c605981ca336ec9)

---

_Label `bug` added by @sharkdp on 2025-09-17 08:26_

---

_Label `generics` added by @sharkdp on 2025-09-17 08:26_

---

_Assigned to @sharkdp by @sharkdp on 2025-09-17 08:26_

---

_Comment by @sharkdp on 2025-09-17 08:31_

This can be reproduced on `main`:

```py
from typing import Self


class C[T]:
    x: T

    def get(self: Self):
        pass


def _(obj: object):
    if isinstance(obj, C):
        obj.get()
```
https://play.ty.dev/3271b09e-7989-4bb2-a185-f0529b889092

```
Argument to bound method `get` is incorrect: Argument type `Top[C[Unknown]]` does not satisfy
upper bound `Bottom[C[Unknown]]` of type variable `Self`

Argument to bound method `get` is incorrect: Expected `Self@get`, found `Top[C[Unknown]]`
```


---

_Comment by @sharkdp on 2025-09-17 11:06_

I think this happens because we apply the flipped materialization kind to parameters of a signature [here](https://github.com/astral-sh/ruff/qblob/7e464b815096f66ad33111f95147184db84ac638/crates/ty_python_semantic/src/types/signatures.rs#L478-L480).

And so, when we access `get` on `Top[C[Unknown]]`, we end up with the bottom materialization on the upper bound of the `self: Self` parameter.


---

_Comment by @sharkdp on 2025-09-17 11:27_

And it only happens for invariant typevars (try changing to co/contra-variant here: https://play.ty.dev/6a77ba03-c87f-45ca-9f09-6c40f02305dd) when this code path is active:

https://github.com/astral-sh/ruff/blob/7e464b815096f66ad33111f95147184db84ac638/crates/ty_python_semantic/src/types/generics.rs#L762-L769

---

_Comment by @sharkdp on 2025-09-17 11:46_

I think this is also related to Doug's comment in https://github.com/astral-sh/ty/issues/1164

---

_Comment by @sharkdp on 2025-09-17 12:15_

Now I'm starting to think that the problem is this code here, where we materialize the type of an attribute when accessing it on `Top[…]` or `Bottom[…]`:

https://github.com/astral-sh/ruff/blob/7e464b815096f66ad33111f95147184db84ac638/crates/ty_python_semantic/src/types.rs#L2810-L2817

This leads us to top-materialize `fn get(self: Self{bound=C[Unknown]})` to `fn get(self: Self{bound=Bottom[C[Unknown]]})`, after which we can't call it (create a bound method object).

---

_Comment by @carljm on 2025-09-17 15:00_

It only happens for invariant type vars because that's the only case where we have to explicitly represent a `Top` or `Bottom` type; otherwise we can just immediately apply the top/bottom materialization. For `Covariant[T]` the top materialization of `Covariant[Any]` is just `Covariant[object]` and the bottom materialization is `Covariant[Never]`; for `Contravariant[T]` it's the reverse. But for invariant type vars we need `Top[Invariant[Any]]`, which represents the infinite union of all possible materializations of `Invariant[Any]`, and `Bottom[Invariant[Any]]`, representing the infinite intersection of all possible materializations. Because of invariance, neither of these is the same type as `Invariant[object]` or `Invariant[Never]`.

Gradual upper bounds are weird, and I suspect that the answer here might be that we should never create a gradual upper bound for `Self` in the first place. If we are initially creating the `Self` type var with an upper bound of `C[Unknown]` in your example, we should instead be initially creating it with the upper bound as the top materialization, so `Top[C[Unknown]]`. This is the correct upper bound -- it is the type that includes all possible materializations of `C`, which are all valid types for `Self` to take on. And since that is already a fully static type, the materialization-of-attributes code won't affect it.



---

_Comment by @JelleZijlstra on 2025-09-17 15:11_

I believe a gradual upper bound is equivalent to an upper bound of the `Top[]` of that gradual upper bound, though I haven't fully worked out that that always holds.

---

_Comment by @carljm on 2025-09-17 16:09_

Some notes from offline discussion:

> I believe a gradual upper bound is equivalent to an upper bound of the `Top[]` of that gradual upper bound

This isn't generally true, at least not internally to the function, where uses of a typevar with upper bound `Any` needs to be treated more forgivingly than a typevar with upper bound `object`. It may be true as far as external solving of the typevar goes?

> we should instead be initially creating it with the upper bound as the top materialization, so `Top[C[Unknown]]`

@sharkdp observed that this isn't right either, because then for internal uses of `self` we have specialized away the typevar of `C` too quickly:

```py
from typing import Self

class C[T]:
    x: T

    def get(self: Self) -> T:
        return self.x  # expected `T@C`, found `object`
```

Effectively it seems that `Self` on a generic type must be treated as a typevar with a generic upper bound of `C[T]`, at least internally to the function, but generic upper bounds are not generally allowed. So that either will require some special-casing of `Self` relative to other typevars, or generally supporting generic upper bounds, which [has been suggested before](https://discuss.python.org/t/union-of-invariant-types/103344/17).

In the short term, we may want a simpler workaround which accepts some false negatives in order to eliminate false positives and allows us to land type-of-self, e.g. not enforcing upper bound of `Self`.

---

_Comment by @sharkdp on 2025-09-18 08:30_

For completeness, here is how our current behavior of using `C[Unknown]` as the upper bound for `Self` can lead to false negatives inside method bodies:
```py
from typing import Self


class C[T: str]:
    attr: T

    def f(self: Self):
        self.attr.replace("foo", 2)  # should be an error
        reveal_type(self.attr)  # ty: Unknown
```
(https://play.ty.dev/45237f59-91c8-4ad7-982b-91a9d73667a2)

---

_Closed by @sharkdp on 2025-09-23 12:02_

---
