```yaml
number: 2572
title: "Result of `max` with `key=len` is always `Sized`"
type: issue
state: open
author: druzsan
labels: []
assignees: []
created_at: 2026-01-20T22:22:14Z
updated_at: 2026-01-20T23:00:34Z
url: https://github.com/astral-sh/ty/issues/2572
synced_at: 2026-01-20T23:51:27Z
```

# Result of `max` with `key=len` is always `Sized`

---

_@druzsan_

### Summary

In the following code I search the longest element in a collection (I tried values of a dict and list):
```python
xs: list[list[str]] = [["foo", "bar"], ["baz", "qux", "quux"]]

x = max(xs, key=len)
reveal_type(x)
```

I expect `x` to be revealed as `list[str]`, but it is revealed as `Sized`.

Am I missing something?

Command executed:
```sh
ty check --error-on-warning
```

No other settings are applied to `ty`.

### Version

0.0.12

---

_Comment by @MeGaGiGaGon on 2026-01-20 23:00_

This happens because the selected overload of `max` for this case is:
```py
@overload
def max(iterable: Iterable[_T], /, *, key: Callable[[_T], SupportsRichComparison]) -> _T: ...
```
And where ty is going wrong is in solving for `_T` - `xs` is `list[list[str]]`, but `len` is defined as:
```py
def len(obj: Sized, /) -> int: ...
```
Which is a `Callable[[Sized], SupportsRichComparison]`, so ty ends up assigning `Sized` to `_T` instead.
(And just for context, `Sized` is this):
```py
@runtime_checkable
class Sized(Protocol, metaclass=ABCMeta):
    @abstractmethod
    def __len__(self) -> int: ...
```
So in this case ty should prefer solving to the concrete class instead of the `Protocol`, but doesn't.

---
