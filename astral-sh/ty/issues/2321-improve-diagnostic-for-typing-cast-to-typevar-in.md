```yaml
number: 2321
title: "improve diagnostic for `typing.cast` to TypeVar in parameter default"
type: issue
state: open
author: JasonGrace2282
labels:
  - diagnostics
assignees: []
created_at: 2026-01-04T20:09:01Z
updated_at: 2026-01-04T22:34:56Z
url: https://github.com/astral-sh/ty/issues/2321
synced_at: 2026-01-12T15:54:26Z
```

# improve diagnostic for `typing.cast` to TypeVar in parameter default

---

_@JasonGrace2282_

### Summary

The following [playground](https://play.ty.dev/d1ed0fb4-3fb8-4c82-ba65-bac5cc7e5be7) gives a diagnostic of
```
Default value of type `TypeVar` is not assignable to annotated parameter type `D@foo`
```
for the following code:
```py
from typing import TypeVar, cast
D = TypeVar('D')

def foo(x: D = cast(D, None)) -> D:
    return x
```

I'm not quite sure if this should be allowed, or if this usage of `cast` should give off an error, but it would be nice if there was either a correct inference or a better error message!

### Version

_No response_

---

_Comment by @quencs on 2026-01-04 20:16_

To clarify, this doesn’t seem like an issue with typing.cast itself.
A default value for a TypeVar-typed parameter must be valid for all possible bindings of that TypeVar, and cast(D, None) doesn’t change that.
The behavior seems reasonable, but the diagnostic could do a better job explaining why this is rejected.

---

_Label `diagnostics` added by @carljm on 2026-01-04 22:34_

---

_Renamed from "`typing.cast` not working with TypeVar's in default parameters" to "improve diagnostic for `typing.cast` to TypeVar in parameter default" by @carljm on 2026-01-04 22:34_

---
