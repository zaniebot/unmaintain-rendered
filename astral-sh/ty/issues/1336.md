```yaml
number: 1336
title: "error when returning dict[str, object] for function declared to return dict[str, str]"
type: issue
state: closed
author: gjcarneiro
labels: []
assignees: []
created_at: 2025-10-10T18:03:43Z
updated_at: 2025-10-10T18:30:36Z
url: https://github.com/astral-sh/ty/issues/1336
synced_at: 2026-01-10T02:06:25Z
```

# error when returning dict[str, object] for function declared to return dict[str, str]

---

_Issue opened by @gjcarneiro on 2025-10-10 18:03_

### Summary

```py
def foo() -> dict[str, object]:
    retval: dict[str, str] = {"foo": "bar"}
    return retval

def main() -> None:
    print(foo())
```

> Return type does not match returned value: expected `dict[str, object]`, found `dict[str, str]` (invalid-return-type) [Ln 3, Col 12]

https://play.ty.dev/e9610be1-3477-4e99-9832-1da48c8f7ca4

Using `object` here as a safer alternative to using `typing.Any`: "Any" is compatible with anything, which can be a problem if you want to catch errors.

Type `str`  is a subtype of `object`, so I don't think this should be an error?

### Version

`0.0.1-alpha.22`
(previous version didn't complain about this)

---

_Comment by @carljm on 2025-10-10 18:17_

This is an error (and all Python type checkers agree on this) because dictionaries are invariant in their value type, not covariant. That means that while `str` is a subtype of `object`, `dict[str, str]` is not a subtype of `dict[str, object]`. The reason is that dictionaries are mutable, so `dict[str, object]` can accept any object in its `__setitem__` method, while `dict[str, str]` can accept only strings; this means `dict[str, str]` cannot be a safe substitute where `dict[str, object]` is expected.

It's less of a practical issue in your particular case (a return value) because the function `foo` doesn't hold on to a reference to the dictionary after it is returned, but we can easily imagine a slightly different case where the returned value is aliased, and it would be unsound to allow this:

```py
def foo(x: dict[str, str]) -> dict[str, object]:
    return x  # this is an error, but let's pretend it wasn't

d: dict[str, str] = {"foo": "bar"}
d2 = foo(d)
d2["baz"] = 1
# now `d` is still typed as `dict[str, str]` but includes a value of type `int`
```

---

_Closed by @carljm on 2025-10-10 18:17_

---

_Comment by @gjcarneiro on 2025-10-10 18:27_

Ah, it seems you're right, even mypy agrees it's an error.  Strange that this codebase, which used to be check by mypy, then switched to `ty 0.0.1-alpha.21`, didn't previously flag this as an error, until the upgrade to `ty 0.0.1-alpha.22`.  Thanks for checking, though!

---

_Comment by @carljm on 2025-10-10 18:30_

Older ty versions didn't flag this because we inferred `dict[Unknown, Unknown]` for all dict literals, which was clearly not ideal :) and we've now fixed that. 

Not sure why mypy wouldn't have previously flagged it, but if you happen to figure out why, and it represents a missing feature in ty, we'd like to know about it!

---
