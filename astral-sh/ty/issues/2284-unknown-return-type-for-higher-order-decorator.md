```yaml
number: 2284
title: "`Unknown` return type for higher order decorator wrapped functions"
type: issue
state: closed
author: jamestrew
labels: []
assignees: []
created_at: 2025-12-31T02:52:55Z
updated_at: 2025-12-31T11:11:54Z
url: https://github.com/astral-sh/ty/issues/2284
synced_at: 2026-01-12T15:54:26Z
```

# `Unknown` return type for higher order decorator wrapped functions

---

_@jamestrew_

### Summary

Higher order decorator = second order decorator = decorator factory
Not sure what the exactly correct term but hope the playground link clears it up. Feel free to change the issue title.

When a function is wrapped by such a decorator, the wrapped functions return type becomes `Unknown`.

```python
from typing import Callable, TypeVar, reveal_type, ParamSpec
from functools import wraps

P = ParamSpec("P")
R = TypeVar("R")


def big_wrap() -> Callable[[Callable[P, R]], Callable[P, R]]:
    def decorator(func: Callable[P, R]) -> Callable[P, R]:
        @wraps(func)
        def wrapper(*args: P.args, **kwargs: P.kwargs) -> R:
            return func(*args, **kwargs)

        return wrapper

    return decorator


@big_wrap()
def wrapped_foo() -> int:
    return 42


x = wrapped_foo()
reveal_type(x)  # Revealed type: `Unknown`
```

https://play.ty.dev/8743f5e8-d4a2-432e-91be-7cd5d25da734

For ref:
[mypy playground](https://mypy-play.net/?mypy=latest&python=3.12&gist=bece442ac4db1b2897138cfd1bb83b7e)
[pyright playgroud](https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoDCAhgDYlEBGJApgDRQAqi1AakSPSNQG7WkD68BHSgAFdkQgBlYQGMAUKEhRgAVxSyYYMCQDOmbLnwB3EEQS7580VAC8YidLkAKAESjXASnkAlO42Y2EDcfLyt5ABNqYCgKJDR%2BU3NnTygAWgA%2BQlJyKmoAbXziMkoafNF6HwBdKvpi3LKKqGqqgC55KE6oKJio2VwiLWC1DVbskrzyyqrUzPGGgqbq9q7VqAABJItnEdlvNa6eqC3hYIAqdjRdMdEAOkvdejOzgGtjB5vbt4fZrJ8Vg5rLgwVQgFAqdSyZwXEBXJ6vd6w3TeDqA4Gg8EnaggKyrdFg7rUfpmIbhdZxBJbFKRaLHMwIYQRfjAbQpdJZVAwAGdfHggAsACZwgAPfxYpkssDUri8ARCajOYWpKAAYmaPD4NAisGYYwABgBVFAvFBgYwoPXyIA)
Both reveal `int` as the type as expected.



### Version

ty ruff/0.14.10+165 (f61978306 2025-12-30)

---

_Comment by @AlexWaygood on 2025-12-31 11:11_

Thanks! I'm pretty sure this is #1136

---

_Closed by @AlexWaygood on 2025-12-31 11:11_

---
