```yaml
number: 2574
title: "support a `Module` type in `ty_extensions` for typing specific modules"
type: issue
state: open
author: KotlinIsland
labels: []
assignees: []
created_at: 2026-01-21T01:57:23Z
updated_at: 2026-01-21T09:48:47Z
url: https://github.com/astral-sh/ty/issues/2574
synced_at: 2026-01-21T10:01:34Z
```

# support a `Module` type in `ty_extensions` for typing specific modules

---

_@KotlinIsland_

### Summary

```py
from ty_extensions import Module


def get_module() ->  Module["collections.abc"]:
    from collections import abc
    
    return abc
```

---

_Comment by @AlexWaygood on 2026-01-21 08:24_

Can you give a more full example of a situation in which you might use this? :-) are you thinking that you might use this to annotate functions that lazy-load expensive modules to reduce import times?

---

_Comment by @augustfe on 2026-01-21 09:46_

It's maybe not the same use case, but it shows up in the `jax` library, causing funky decorator behaviour with their `@export`,

https://github.com/jax-ml/jax/blob/c4481d422bc9c0e4816dd8d895709e9c27be1f8c/jax/_src/nn/initializers.py#L39
```python
export = set_module('jax.nn.initializers')
```

based on this method

https://github.com/jax-ml/jax/blob/c4481d422bc9c0e4816dd8d895709e9c27be1f8c/jax/_src/util.py#L777-L782
```python
def set_module(module: str) -> Callable[[T], T]:
  def wrapper(func: T) -> T:
    if module is not None:
      func.__module__ = module
    return func
  return wrapper
```

---

_Comment by @AlexWaygood on 2026-01-21 09:48_

@augustfe I'm not sure that's related at all (I think your issue is  https://github.com/astral-sh/ty/issues/1136)

---
