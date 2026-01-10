```yaml
number: 2380
title: dict erroneously does not match a Protocol with get()
type: issue
state: closed
author: bcmills
labels: []
assignees: []
created_at: 2026-01-07T20:47:36Z
updated_at: 2026-01-07T23:02:12Z
url: https://github.com/astral-sh/ty/issues/2380
synced_at: 2026-01-10T01:56:41Z
```

# dict erroneously does not match a Protocol with get()

---

_Issue opened by @bcmills on 2026-01-07 20:47_

### Summary

https://play.ty.dev/d9fa963a-91ff-4a6d-a39d-53add13eeeee:
```py
from typing import Protocol

class _Headers(Protocol):
    def get(self, /, key: str) -> str| None: ...

d: dict[str, str] = {}

def use_dict(h: dict[str, str]):
    v: str | None = h.get("foo")

def use_headers(h: _Headers):
    v: str | None = h.get("foo")

use_dict(d)
use_headers(d)

```

`ty` accepts both `use_dict` and `use_headers` as valid functions, and when running this program directly in Python, both `use_dict(d)` and `use_headers(d)` succeed without type errors.

However, `ty` rejects the use of a `dict` as an instance of `Headers`:
```
Argument to function `use_headers` is incorrect: Expected `_Headers`, found `dict[str, str]` (invalid-argument-type) [Ln 15, Col 13]
```

It looks like this may be a bad interaction between Protocol and overload resolution.

### Version

ty 0.0.9

---

_Comment by @carljm on 2026-01-07 23:01_

Hi, thanks for the report!

ty is correct here, and pyright and pyrefly agree -- only mypy allows this, apparently because it doesn't fully consider positional-only arguments in callable assignability.

The key factor is that all overloads of `dict.get` have all arguments marked as positional-only in typeshed, meaning that a dict `d` does not permit the call `d.get(key="foo")`, but your `_Headers` protocol currently does (if you try changing the `.get` calls in both `use_dict` and `use_headers` to use a keyword argument, you'll see that only the one in `use_dict` fails.)

If you change the signature of `get` on your `_Headers` protocol to `def get(self, key: str, /) -> str| None: ...` (making the `key` argument positional-only, as on `dict.get`), then ty is happy with this code (and so are all other type checkers).

---

_Closed by @carljm on 2026-01-07 23:01_

---
