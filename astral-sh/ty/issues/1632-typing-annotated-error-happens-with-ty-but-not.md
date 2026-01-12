```yaml
number: 1632
title: "`typing.Annotated` error happens with `ty` but not `pyright` with `jaxtyping`"
type: issue
state: closed
author: toddpocuca
labels:
  - library
assignees: []
created_at: 2025-11-25T19:11:50Z
updated_at: 2025-11-25T21:30:00Z
url: https://github.com/astral-sh/ty/issues/1632
synced_at: 2026-01-12T15:54:25Z
```

# `typing.Annotated` error happens with `ty` but not `pyright` with `jaxtyping`

---

_@toddpocuca_

### Summary

```py
from jaxtyping import Key
import jax.random as jr

def hi(key: Key = jr.key(0)) -> Key:
    return key
```
```
error[invalid-type-form]: `typing.Annotated` requires at least two arguments when used in a type expression
  --> tests/inference/test_continuous.py:82:13
   |
81 | # `typing.Annotated` requires at least two arguments when used in a type expression (ty invalid-type-form)
82 | def hi(key: Key = jr.key(0)) -> Key:
   |             ^^^
83 |     return key
   |
info: rule `invalid-type-form` is enabled by default

error[invalid-type-form]: `typing.Annotated` requires at least two arguments when used in a type expression
  --> tests/inference/test_continuous.py:82:33
   |
81 | # `typing.Annotated` requires at least two arguments when used in a type expression (ty invalid-type-form)
82 | def hi(key: Key = jr.key(0)) -> Key:
   |                                 ^^^
83 |     return key
   |
info: rule `invalid-type-form` is enabled by default
```
I'm not sure what's going on, but I've never had an issue with `jaxtyping` before.

### Version

ty 0.0.1-alpha.27 (26d7b6864 2025-11-18)

---

_Comment by @carljm on 2025-11-25 20:52_

Hmm, interesting. Thanks for the report!

I think the diagnostic we are giving here is correct. Mypy gives the same diagnostic, and so does pyright, but only in strict mode. `jaxtyping.Key` (along with many other types in `jaxtyping`) is just an alias for `typing.Annotated` (via a couple import indirections). And `typing.Annotated` normally requires two arguments when used in a type annotation (e.g. `Annotated[SomeType, "some annotation"]`).

I wonder why all the jaxtyping types are aliases to `Annotated` if they are supposed to be used "bare" like this? Why aren't they e.g. aliases to `typing.Any` instead? (If you annotate something as `jaxtyping.Key` and check it in pyright non-strict mode, it's just being typed as `Any` anyway.) Maybe @patrick-kidger can shed some light on the intent here?

I think we will continue to error on this by default, but if this is a widely-used pattern we could perhaps use a more fine-grained error code than just `invalid-type-form`, so projects using jaxtyping can disable this error?

---

_Label `library` added by @carljm on 2025-11-25 20:52_

---

_Comment by @patrick-kidger on 2025-11-25 21:28_

This is not the intended use of jaxtyping :) `Key` on its own does not mean anything. jaxtyping type hints require both an array and a shape, e.g. `Key[Array, "batch channels"]`.

I suspect in this case @toddpocuca is after specifically a scalar key (like is returned from `jr.key`) which would be `Key[Array, ""]`. Note that vector (or matrix, ...) shaped key arrays can be obtained using `jr.split` or returned from `jax.vmap`.

---

_Comment by @carljm on 2025-11-25 21:29_

Thank you @patrick-kidger !

---

_Comment by @carljm on 2025-11-25 21:30_

Closing as not-planned since the diagnostic is correct, and this isn't the intended use of jaxtyping.

---

_Closed by @carljm on 2025-11-25 21:30_

---
