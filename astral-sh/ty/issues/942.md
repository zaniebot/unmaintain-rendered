```yaml
number: 942
title: Type inference and function transformations
type: issue
state: closed
author: syrkis
labels:
  - question
assignees: []
created_at: 2025-08-05T17:35:43Z
updated_at: 2025-08-05T20:01:26Z
url: https://github.com/astral-sh/ty/issues/942
synced_at: 2026-01-10T02:06:24Z
```

# Type inference and function transformations

---

_Issue opened by @syrkis on 2025-08-05 17:35_

### Question

When coding in, for example Jax, function transformations are common (`vmap` for vectorizing a function across an axis, `grad` for the gradient of a function). `functools.partial` is another example. Currently (it seems to me) `ty` has limited support for this. I've attached an example of a scenario in which type inference should be possible, but is not happening.

<img width="1499" height="1315" alt="Image" src="https://github.com/user-attachments/assets/ce1b7926-2efb-4eee-941d-fe9735502ba7" />


### Version

_No response_

---

_Label `question` added by @syrkis on 2025-08-05 17:35_

---

_Comment by @sharkdp on 2025-08-05 17:50_

Thank you for reporting this.

Can you please share that code as text (if possible)? Ideally, in a self-contained way and minified to the problematic lines.

---

_Comment by @syrkis on 2025-08-05 17:59_

```python
import jax.numpy as jnp
from functools import partial
from jaxtyping import Array

def f(a: Array, b: Array) -> Array:
    return a + b


a = jnp.ones(2)
b = jnp.ones(2)
c = partial(f, a)(b)  # Type is not inferred 
d = f(a, b) # Type IS inferred

```

---

_Comment by @syrkis on 2025-08-05 18:11_

I am using zed.dev as my editor, with inline type hints. Here is another example using numpy instead of jax



```python
import numpy as np
from numpy.typing import NDArray
from functools import partial


def f(a: NDArray[np.float32], b: NDArray[np.float32]) -> NDArray[np.float32]:
    return a + b


a: NDArray[np.float32] = np.ones(2, dtype=np.float32)
b: NDArray[np.float32] = np.ones(2, dtype=np.float32)
d = f(a, b)  # Type is inferred as \@todo
c = partial(f, a)(b)  # Type is infered as unknown

```

---

_Comment by @carljm on 2025-08-05 20:01_

Thanks for the report; you've identified some known limitations that we are working on improving!

The failure to infer a precise type for `partial` is #500 (which may just be wrapped up into @dcreager 's current work on #623).

The todo type for `NDArray` is due to #544, which should be unblocked soon.

---

_Closed by @carljm on 2025-08-05 20:01_

---
