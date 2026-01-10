```yaml
number: 1132
title: "typing.Hashable | typing.Sequence[typing.Hashable] not matching [\"x\"] in ty==0.0.1a20"
type: issue
state: closed
author: darabos
labels: []
assignees: []
created_at: 2025-09-05T11:51:56Z
updated_at: 2025-09-10T10:38:59Z
url: https://github.com/astral-sh/ty/issues/1132
synced_at: 2026-01-10T02:06:25Z
```

# typing.Hashable | typing.Sequence[typing.Hashable] not matching ["x"] in ty==0.0.1a20

---

_Issue opened by @darabos on 2025-09-05 11:51_

### Summary

Code for reproduction:
```python
import typing

a: typing.Hashable = "GOOD"
a: typing.Sequence[typing.Hashable] = ["GOOD"]
a: typing.Hashable | typing.Sequence[typing.Hashable] = "GOOD"
a: typing.Hashable | typing.Sequence[typing.Hashable] = ["FAILS"]
```

With `ty==0.0.1a19` it passes but with `ty==0.0.1a20` the last line fails:

```
error[invalid-assignment]: Object of type `list[@Todo]` is not assignable to `Hashable`
 --> x.py:6:1
  |
4 | a: typing.Sequence[typing.Hashable] = ["GOOD"]
5 | a: typing.Hashable | typing.Sequence[typing.Hashable] = "GOOD"
6 | a: typing.Hashable | typing.Sequence[typing.Hashable] = ["FAILS"]
  | ^
  |
info: rule `invalid-assignment` is enabled by default
```

If I change `Hashable` to `str` or `Sequence` to `list` then it works. But `typing.Hashable | typing.Sequence[typing.Hashable]` is a common type in Pandas. E.g. https://github.com/pandas-dev/pandas/blob/3c14b71c8f965ff316b22dfb3d87839361f55297/pandas/core/frame.py#L6319 I'm not sure how to call those methods with `ty==0.0.1a20` if I want to pass in a list of strings.

### Version

0.0.1a20

---

_Comment by @AlexWaygood on 2025-09-05 12:07_

Thanks for the report!

The issue here is that `Sequence[Hashable]` is (according to all normal rules!) a subtype of `Hashable`: `object` defines a `__hash__` method, therefore `object` is `Hashable`; and `Sequence[Hashable]` is a subtype of `object`, which therefore means that `Sequence[Hashable]` is also `Hashable`. The fact that `Sequence[Hashable]` is a subtype of `Hashable` means that the union `Sequence[Hashable] | Hashable` is redundant; ty eagerly simplifies this union to simply `Hashable`.

Since all types in Python are subtypes of `object`, and `object` is a subtype of `Hashable`, this implies that all objects in Python are subtypes of `Hashable`, which is... absurd -- that's obviously not true! This is a tough problem to solve, however, since the rules around hashability in Python disobey all normal rules a type checker generally assumes when inferring types and checking code. Unhashable classes often inherit from hashable classes, and hashable classes often inherit from unhashable classes; this all deeply violates the Liskov Substitution Principle. There's a comment to this effect in typeshed's stubs here: https://github.com/astral-sh/ruff/blob/7ee863b6d7df2757a16dbbff59ca5a04b993d57a/crates/ty_vendored/vendor/typeshed/stdlib/typing.pyi#L869-L875

The problematic nature of `Hashable` affects ty much more severely than it affects other type checkers, because of the fact that ty will eagerly simplify redundant unions; other type checkers generally only simplify unions lazily. If we left the union unsimplified here, we would recognize `list[str]` as being a subtype of `Sequence[Hashable] | Hashable` -- it is a subtype of the first element of the union but not the second, even though the first is a subtype of the second. (Subtyping is not necessarily transitive when the Liskov principle is violated!) We're considering doing less eager union simplification in some situations; that might help this issue. Another possible solution here is to eagerly simplify `Hashable` to `object` (since both types -- in theory -- represent the set of all possible Python objects, they're the same type under all normal rules!). That would fix the false positive here (`list[str]` will easily be recognised by ty as a subtype of `object`), but might produce some unexpected behaviour in other situations.

I also really wish pandas would use `Hashable` less in their annotations, though... even if we fix this behaviour in ty so that we have more intuitive behaviour, there's no way to use `Hashable` in a type annotation that won't cause unfortunate surprises and unexpected behaviour in some situations. I've had this conversation with them [before](https://github.com/PyCQA/flake8-pyi/issues/283#issuecomment-1248095891).

---

_Comment by @carljm on 2025-09-05 17:08_

I think in the short term we should simplify `Hashable` to `object`. That is correct, according to the definitions of `object` and `Hashable`. That should eliminate false positives from this issue. What it gives up is type-checking of things actually being hashable, but the whole idea that the Python type system can safely provide that today is a lie anyway.

---

_Closed by @AlexWaygood on 2025-09-10 10:38_

---
