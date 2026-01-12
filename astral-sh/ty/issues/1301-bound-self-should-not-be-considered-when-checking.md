```yaml
number: 1301
title: "Bound `self` should not be considered when checking assignability of bound methods"
type: issue
state: closed
author: dcreager
labels: []
assignees: []
created_at: 2025-10-03T13:51:02Z
updated_at: 2025-10-03T15:15:59Z
url: https://github.com/astral-sh/ty/issues/1301
synced_at: 2026-01-12T15:54:24Z
```

# Bound `self` should not be considered when checking assignability of bound methods

---

_@dcreager_

This seems related to #1169 and #1172, but slightly different and more general. It does not require overloads, nor having other parameters or a return value that use `typing.Self`.

We are currently not performing assignability/subtyping checks on bound methods correctly [[playground](https://play.ty.dev/11d587b4-4f0c-450a-a180-52b2c35edc91)]:

```py
# These succeed, as they should, since return types are in covariant position.
static_assert(is_subtype_of(Callable[[], int], Callable[[], object]))
static_assert(is_subtype_of(Callable[[], Iterator[int]], Callable[[], Iterator[object]]))

class I:
    def method1(self) -> int: ...
    def method2(self) -> Iterator[int]: ...

class O:
    def method1(self) -> object: ...
    def method2(self) -> Iterator[object]: ...

# These fail
reveal_type(I().method1)
reveal_type(O().method1)
static_assert(is_subtype_of(TypeOf[I().method1], TypeOf[O().method1]))
static_assert(is_subtype_of(TypeOf[I().method2], TypeOf[O().method2]))
```

Some printf-debugging shows that we are checking the `self` parameter when doing the assignability check. In this case, since parameters are in a contravariant position, we are deciding that `Self: int` is not a ~subtype~ supertype of `Self: object`, and therefore causing the overall subtyping check to fail.

In a bound method, the bound `self` parameter has been curried away, and so shouldn't be considered at all when determining subtyping of the bound method.

---

_Assigned to @dcreager by @dcreager on 2025-10-03 13:51_

---

_Comment by @dcreager on 2025-10-03 15:15_

@AlexWaygood convinced me synchronously that this is wrong! The bound methods of two different classes really are different sets of objects, and are correctly not subtypes of each other. It was a different issue that I was encountering on #20677.

---

_Closed by @dcreager on 2025-10-03 15:15_

---
