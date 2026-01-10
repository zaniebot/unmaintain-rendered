```yaml
number: 504
title: "Option for not stripping `Annotated` metadata."
type: issue
state: open
author: carrascomj
labels:
  - server
  - wish
assignees: []
created_at: 2025-05-24T21:24:33Z
updated_at: 2025-12-06T01:17:50Z
url: https://github.com/astral-sh/ty/issues/504
synced_at: 2026-01-10T01:56:40Z
```

# Option for not stripping `Annotated` metadata.

---

_Issue opened by @carrascomj on 2025-05-24 21:24_

As a user, I would like to be able to see the [`Annotated` ](https://docs.python.org/3/library/typing.html#typing.Annotated)metadata on hover.

### Current behavior

The first argument is correctly interpreted as the type but the metadata is stripped on hover.

![Image](https://github.com/user-attachments/assets/ba249ae0-62d4-4fe9-8609-37c149887f56)

### Desired behavior

The user should be able to configure `ty` to use it (maybe not by default). My particular use case is `jaxtyping` for annotating tensor shapes. If I use `ty` (or `pyright`, which [likely won't implement it](https://github.com/microsoft/pyright/discussions/6528#discussioncomment-7654251)), I don't see the shape on hover, which is very helpful for programming with tensors in [pytorch](https://pytorch.org/), [jax](https://docs.jax.dev/en/latest/), etc.

For example, (this is with pyright):

![Image](https://github.com/user-attachments/assets/d4a49ac1-b64e-4041-8418-618a764e910f)

---

_Label `wish` added by @carljm on 2025-05-27 18:39_

---

_Label `server` added by @carljm on 2025-05-27 18:39_

---

_Comment by @MichaReiser on 2025-05-28 09:14_

@carljm That would probably require fundamental changes to how we represent `Annotated` that go beyond just the LSP. 

---

_Comment by @AlexWaygood on 2025-05-28 09:17_

> [@carljm](https://github.com/carljm) That would probably require fundamental changes to how we represent `Annotated` that go beyond just the LSP.

I would guess the reason why Carl added the `server` label is because this is a situation where the needs of a type checker and the needs of an LSP server are slightly in tension with each other. For a command-line type checker, it makes sense to eagerly strip the metadata, since it's useless for type checking. If we changed our model to retain the metadata (which would significantly complicate our internal representation of the type, as you say), it would solely be because of the use cases the LSP server might have for it (on-hover functionality, etc.)

---

_Comment by @carljm on 2025-05-28 17:59_

Yes, Alex is right -- I marked this `server` because it's a change that would be motivated entirely by LSP server use cases, not because the required code changes would be strictly limited to the LSP server code.

(I don't think the required code changes would be _super_ invasive, though. I don't think we would track the `Annotated` data as part of the type, just as another kind of type qualifier. Which implies that it wouldn't "travel" with the type, it would apply just to the particular annotated name -- but I think that's preferable for most use cases anyway.)

---

_Comment by @mrdmnd on 2025-06-23 16:17_

Flying by to add that I would also love this change too!

---

_Comment by @carrascomj on 2025-11-30 23:23_

I have been experimenting with this, and I think the best option is to have a specialized language server to handle jaxtyping annotations, since the experience I was looking for was to ultimately have static shape analysis to report errors as diagnostics and run shape inference on hover.

I have made my own ([shapels](https://github.com/carrascomj/shapels), only for torch), feel free to close this if it's not of general interest.

---

_Comment by @jkyl on 2025-12-06 01:17_

im a user of jaxtyping who'd love to see shape metadata preserved! another relevant use case of `Annoted` might be to extend jaxtyping with [sharding information](https://docs.jax.dev/en/latest/notebooks/explicit-sharding.html), something like:
```python
import jax
from jax.sharding import PartitionSpec as P
from jax.sharding import NamedSharding, Sharding
from typing import Annotated
from beartype.vale import Is
from jaxtyping import Array, Float32

class Sharded:
    """Extends jaxtyping.Array with sharding info."""
    def __class_getitem__(cls, args: tuple[type, str, Sharding]):
        dtype, shape, sharding = args
        return Annotated[
            dtype[Array, shape],
            Is(lambda x: jax.typeof(x).sharding == sharding)
        ]

mesh = jax.make_mesh((8,), ("data",)) 
sharding = NamedSharding(mesh, P("data"))
array: Sharded[Float32, "M N", sharding] = jnp.ones((64, 64), out_sharding=sharding)

```

---
