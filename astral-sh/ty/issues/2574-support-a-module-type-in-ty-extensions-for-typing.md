```yaml
number: 2574
title: "support a `Module` type in `ty_extensions` for typing specific modules"
type: issue
state: open
author: KotlinIsland
labels:
  - needs-decision
  - wish
assignees: []
created_at: 2026-01-21T01:57:23Z
updated_at: 2026-01-21T11:57:23Z
url: https://github.com/astral-sh/ty/issues/2574
synced_at: 2026-01-21T12:56:36Z
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

_Comment by @jorenham on 2026-01-21 11:25_

I like this 👌. And for what it's worth; the pyrights do something similar:

```py
import math
from typing import reveal_type

reveal_type(math)  # Type of "math" is "Module("math")"
```

https://basedpyright.com/?typeCheckingMode=strict&code=JYWwDg9gTgLgBCAhjAFgKAGZQiOMCeYwAdgOZyiSxxQCmAbrYgDYD6BYtaadjL7hWgAokqAJRw4AYjgAVQXAgY4AIlEoVFAM6qAshAAmAV2bC1yDWJVogA

---

_Comment by @AlexWaygood on 2026-01-21 11:31_

> I like this 👌. And for what it's worth; the pyrights do something similar:

Right, we already do that too, but I don't believe pyright provides what this feature request is asking for, which is a way of expressing that "module-literal" type in user annotations. (Don't know about basedpyright.)

> are you thinking that you might use this to annotate functions that lazy-load expensive modules to reduce import times?

@KotlinIsland, you upvoted this comment, so I assume the answer is "yes". This wouldn't cover all cases where you use functions to lazy-load modules -- e.g. [functions that return specific functions or classes from a lazily loaded module](https://github.com/python/cpython/blob/4ef30a5871db2043712945e64f33af81938d68dc/Lib/typing.py#L1930-L1935) -- we would need to provide a way for users to express function-literal types and class-literal types too in order to be able to annotate those functions. But in the meantime, I guess you can always annotate those functions as returning `Callable`/`type[]` types (though these are not as precise as function-literal and class-literal types).

---

_Label `wish` added by @AlexWaygood on 2026-01-21 11:32_

---

_Comment by @jorenham on 2026-01-21 11:32_

As for a use-case, [mpmath](https://github.com/mpmath/mpmath) comes to mind, which [sympy](https://github.com/sympy/sympy) heavily relies on. It uses module-level polymorphism, and they refer to a polymorphic modules as a "context" ([docs](https://mpmath.readthedocs.io/en/latest/contexts.html)). 
At one point I attempted writing stubs for it, but I wasn't able to get very far because of this. But with the proposed `Module` type I think this should be possible.

---

_Comment by @AlexWaygood on 2026-01-21 11:33_

Ah yeah, you can always annotate module-literal types using `Protocol`s for that kind of thing but that gets painful pretty quickly. That's definitely a reasonable use case.

---

_Comment by @KotlinIsland on 2026-01-21 11:44_

> Ah yeah, you can always annotate module-literal types using `Protocol`s for that kind of thing but that gets painful pretty quickly. That's definitely a reasonable use case.

yes! i've run into this before, same with lazy loading mentioned above with some painful import cycles

i wonder if there's any value in a special form that sucks the nominality off something, a little like `CallableTypeOf`, and creates a protocol from it

```py
from ty_extensions import ProtocolOf

class A:
    a: int

class B:
    a: int

type P = ProtocolOf[A]

p: P = B()
```


---

_Comment by @dangotbanned on 2026-01-21 11:51_

Just for another example, in (https://github.com/narwhals-dev/narwhals) we'd really benefit from this since all dependencies are optional.

For some internal things where we can eat the breakage in typing, something like this hack (https://github.com/narwhals-dev/narwhals/pull/2051#discussion_r1969758247) can kinda work.

But we have [a bunch of public functions](https://github.com/narwhals-dev/narwhals/blob/e4c63f8f7c49a2325174ee78223970607d3c3d08/narwhals/dependencies.py#L47-L126) that return entire packages, which:
1. Would be unrealistic to stub out correctly for even 1 package (let alone 16! 😭)
2. Needs to be expressed as a `Module("package") | None`, so can't rely on the inference hack we have elsewhere

---

_Comment by @AlexWaygood on 2026-01-21 11:52_

Oh, I forgot -- we actually do already have a way of annotating module-literal types; I don't know if this is sufficient?

```py
from __future__ import annotations

from typing import TYPE_CHECKING

if TYPE_CHECKING:
    import collections.abc
    from ty_extensions import TypeOf

def get_module() -> TypeOf[collections.abc]:
    import collections.abc
    return collections.abc
```

---

_Label `needs-decision` added by @AlexWaygood on 2026-01-21 11:53_

---
