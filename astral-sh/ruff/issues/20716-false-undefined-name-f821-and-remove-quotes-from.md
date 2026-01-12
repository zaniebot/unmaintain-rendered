```yaml
number: 20716
title: False undefined name (F821) and remove quotes from type annotation (UP037) when string is expected
type: issue
state: open
author: ShaiAvr
labels:
  - bug
  - type-inference
assignees: []
created_at: 2025-10-06T10:09:57Z
updated_at: 2025-12-10T15:52:11Z
url: https://github.com/astral-sh/ruff/issues/20716
synced_at: 2026-01-12T15:54:57Z
```

# False undefined name (F821) and remove quotes from type annotation (UP037) when string is expected

---

_@ShaiAvr_

### Summary

I use the `astropy` library, which defines a `Quantity` object that represents a value with units. According to their [docs](https://docs.astropy.org/en/latest/units/type_hints.html), I can also annotate variables to indicate that they're quantities and specify their units or physical type:
```python
# foo.py
from __future__ import annotations

import astropy.units as u

x: u.Quantity[u.km] = 5 * u.km  # Annotate variable with unit of kilometers
# Annotate variable with dimension of length. Any unit of length is valid.
y: u.Quantity["length"] = 300 * u.m
print(x + y)  # Results in 5.3 km
```
The second form is more natural in my opinion, since it doesn't restrict me to a specific unit, and clearly indicates that the variable `y` represents distance and any units of distance (For example: meters, kilometers, parsecs, etc) are valid. However, `ruff` isn't happy about this code. With the default configuration (no `pyproject.toml` involved), running `ruff check foo.py` results in `F821` error since the name `length` is undefined. If I enable the `UP` rules by running `ruff check --extend-select UP foo.py`, I also get `UP037` warning since I have `from __future__ import annotations`, so `ruff` wants me to remove the quotes, and it's wrong since `"length"` is a literal string.

These errors indicate that `ruff` believes that every parameter inside a type annotation should be a valid type, which is the usual way we annotate: `list[int]`, `Iterable[str]`, so these errors would have made sense on things like `list` or `Iterable`. However, in this case, `Quantity` expects to receive a string parameter, and the annotations in my example are completely valid.

I don't think `ruff` should assume that arguments of type annotations should always be types. Third-party classes may accept literal strings as annotations like here, and `ruff` incorrectly flags that as an error.

### Version

ruff 0.13.3

---

_Comment by @ntBre on 2025-10-06 13:45_

Thanks for the report! This reminds me of some related issues around `Literal` aliasing (https://github.com/astral-sh/ruff/issues/17931, https://github.com/astral-sh/ruff/issues/6014). It looks like `Quantity.__class_getitem__` [returns a `typing.Annotated`](https://github.com/astropy/astropy/blob/c9fe89a65ea9cb75eb00a40dc5bd4895f40af54b/astropy/units/quantity.py#L410). Ruff recognizes the `Annotated` form, so this also seems to be a type inference issue.

---

_Label `bug` added by @ntBre on 2025-10-06 13:45_

---

_Label `type-inference` added by @ntBre on 2025-10-06 13:45_

---

_Comment by @tbekalarek-wrmnd on 2025-12-10 11:19_

I ran into the same problem while using `@strawberry.type` + `Annotated[..., StrawberryPrivate()]` with a forward reference to a type that’s defined later in the module.

Example minimal snippet:
```
from typing import Annotated
from strawberry.private import StrawberryPrivate

@strawberry.type
class MyModel:
    store: Annotated["MyModelStore", StrawberryPrivate()]
```

But when I run Ruff with `UP037` + autofix, it automatically removes the quotes and rewrites it as
```
store: Annotated[MyModelStore, StrawberryPrivate()]
```

which then raises `NameError: name 'MyModelStore' is not defined` at runtime because the class is defined later (or lazily imported).

I currently have to use a dummy alias purely to force Ruff to leave the forward reference quoted:

```
MyModelStoreRef = Annotated["MyModelStore", StrawberryPrivate()]

@strawberry.type
class MyModel:
    store: MyModelStore
```

or disable the rule with `# noqa: UP037`.

---

_Comment by @ntBre on 2025-12-10 15:52_

@tbekalarek-wrmnd That sounds like a different issue to me since your example uses `typing.Annotated`, not an alias that Ruff can't resolve. I think you may want to use [runtime-evaluated-decorators](https://docs.astral.sh/ruff/settings/#lint_flake8-type-checking_runtime-evaluated-decorators) to mark classes decorated with  `strawberry.type` as needing their annotations at runtime. Note that I also had to add `import strawberry`, but configuring this setting silences UP037 here: https://play.ruff.rs/bb8ee1c6-b59b-4064-86e9-0c5a08a3ac2b

---
