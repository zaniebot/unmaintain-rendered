```yaml
number: 2069
title: functool partial and jax jit decorator interfere with argument types of function
type: issue
state: open
author: chris-RNG
labels:
  - generics
assignees: []
created_at: 2025-12-18T13:19:39Z
updated_at: 2025-12-23T21:42:01Z
url: https://github.com/astral-sh/ty/issues/2069
synced_at: 2026-01-12T15:54:26Z
```

# functool partial and jax jit decorator interfere with argument types of function

---

_@chris-RNG_

### Summary

I'm not sure if this is a bug, but since a few ty updates ago, I've started encountering this type error when combining functools.partial with the jax.jit decorator on a function.

```py
from functools import partial

import jax
import jax.numpy as jnp
from jax import Array


@jax.jit
def sum_jit(a:Array, scale:float=1.0)->Array:
    return jnp.sum(a)*scale

@partial(jax.jit, static_argnames=["scale"])
def sum_partial_jit(a:Array, scale:float=1.0)->Array:
    return jnp.sum(a)*scale

def main():
    n=12
    a= jnp.ones(n)
    sum1 = sum_jit(a)
    sum2 = sum_partial_jit(a)
    print(f"sum1: {sum1}")
    print(f"sum1: {sum2}")

if __name__ == "__main__":
    main()
```
```
$ uvx ty check
error[invalid-argument-type]: Argument is incorrect
  --> main.py:20:27
   |
18 |     a= jnp.ones(n)
19 |     sum1 = sum_jit(a)
20 |     sum2= sum_partial_jit(a)
   |                           ^ Expected `(...) -> Unknown`, found `Array`
21 |     print(f"sum1: {sum1}")
22 |     print(f"sum1: {sum2}")
   |
info: Union variant `((...) -> Unknown, /) -> JitWrapped` is incompatible with this call site
info: Attempted to call union type `JitWrapped | (((...) -> Unknown, /) -> JitWrapped)`
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

### Version

ty 0.0.3

---

_Label `generics` added by @carljm on 2025-12-23 21:39_

---

_Added to milestone `Stable` by @carljm on 2025-12-23 21:39_

---

_Comment by @carljm on 2025-12-23 21:42_

Thanks for the report!

---
