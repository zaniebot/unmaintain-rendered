```yaml
number: 18063
title: "False positive for `F722`?"
type: issue
state: closed
author: gbaian10
labels: []
assignees: []
created_at: 2025-05-13T04:46:52Z
updated_at: 2025-05-13T07:09:13Z
url: https://github.com/astral-sh/ruff/issues/18063
synced_at: 2026-01-10T11:09:58Z
```

# False positive for `F722`?

---

_Issue opened by @gbaian10 on 2025-05-13 04:46_

### Summary

I use [jaxtyping](https://github.com/patrick-kidger/jaxtyping) package to do some type checking tasks.

```py
import numpy as np
from jaxtyping import Float32


# For jaxtyping, which is correct content, ruff did not report any errors.
def foo(x: Float32[np.ndarray, "1"]) -> None: ...


# For jaxtyping, which is correct content, ruff reported an `F722` error.
def bar(x: Float32[np.ndarray, "1 2"]) -> None: ...


# For jaxtyping, which is `incorrect` content, ruff did not report any errors.
def baz(x: Float32[np.ndarray, "1, 2"]) -> None: ...
```

In the three examples above, only the second one will report an `F722` error.
The only difference between them is the string content. I’m not sure if this is a false positive.

### Version

ruff 0.11.9

---

_Closed by @MichaReiser on 2025-05-13 07:09_

---
