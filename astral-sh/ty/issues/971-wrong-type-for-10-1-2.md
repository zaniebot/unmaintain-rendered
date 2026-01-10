```yaml
number: 971
title: "wrong type for 10**1.2"
type: issue
state: closed
author: chris-RNG
labels: []
assignees: []
created_at: 2025-08-12T09:12:40Z
updated_at: 2025-11-12T01:56:34Z
url: https://github.com/astral-sh/ty/issues/971
synced_at: 2026-01-10T02:06:24Z
```

# wrong type for 10**1.2

---

_Issue opened by @chris-RNG on 2025-08-12 09:12_

### Summary

The result of an integer to the power of a float returns type int instead of float. The [Minimal Playground Example](https://play.ty.dev/b1951f72-125d-4a63-bd51-bcae07139e2f) shows int instead of float.
If I use it with jax Array, type int is expected instead of Array. If i use a a float like 10.0**1.2 its working as expected.

```python
# /// script
# requires-python = ">=3.13"
# dependencies = ["jax"]
# ///

import jax
from jax import Array


def main() -> None:
    a: Array = jax.numpy.array([1.1, 2.1, 3.1])
    b: int = 10**a
    c: Array = 10**a
    d: Array = 10.0**a
    e: Array = jax.numpy.power(10, a)
    f: int = 10**1.2
    print(f"a: {a}, b: {b}, c: {c}, d: {d}, e: {e}, f: {f}")


if __name__ == "__main__":
    main()

```
Output of ty check:
```
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                          error[invalid-assignment]: Object of type `int` is not assignable to `Array`
  --> ty_issue.py:13:5
   |
11 |     a: Array = jax.numpy.array([1.1, 2.1, 3.1])
12 |     b: int = 10**a
13 |     c: Array = 10**a
   |     ^
14 |     d: Array = 10.0**a
15 |     e: Array = jax.numpy.power(10, a)
   |
info: rule `invalid-assignment` is enabled by default

Found 1 diagnostic
```

### Version

ty 0.0.1-alpha.17

---

_Comment by @AlexWaygood on 2025-08-12 09:17_

Thanks for the report! This is a duplicate of #544 — see https://github.com/astral-sh/ty/issues/616#issuecomment-2955889207 for an explanation of why 

---

_Closed by @AlexWaygood on 2025-08-12 09:17_

---

_Comment by @carljm on 2025-11-12 01:56_

Confirmed this will be fixed by https://github.com/astral-sh/ruff/pull/21394

---
