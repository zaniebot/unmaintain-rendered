```yaml
number: 1160
title: Decorator gives unknown return type
type: issue
state: closed
author: vlashada
labels:
  - question
assignees: []
created_at: 2025-09-10T01:07:38Z
updated_at: 2025-09-10T01:11:23Z
url: https://github.com/astral-sh/ty/issues/1160
synced_at: 2026-01-10T02:06:25Z
```

# Decorator gives unknown return type

---

_Issue opened by @vlashada on 2025-09-10 01:07_

### Question

For the below code, I would expect the typing to be str and not unknown:

From https://github.com/pola-rs/polars/blob/57cbc95bceb6c5e092ada748d27ce81dc908601c/py-polars/polars/io/parquet/functions.py#L403

```python
from functools import wraps
from typing import Callable, ParamSpec, TypeVar, reveal_type

P = ParamSpec("P")
T = TypeVar("T")


def my_decorator(
    old_name: str, new_name: str, *, version: str
) -> Callable[[Callable[P, T]], Callable[P, T]]:
    def decorate(function: Callable[P, T]) -> Callable[P, T]:
        @wraps(function)
        def wrapper(*args: P.args, **kwargs: P.kwargs) -> T:
            return function(*args, **kwargs)

        return wrapper

    return decorate


@my_decorator("row_count_offset", "row_index_offset", version="0.20.4")
def say() -> str:
    return "hello world"


res = say()
reveal_type(res) # Revealed type: `Unknown`
```

### Version

ty 0.0.1-alpha.20

---

_Label `question` added by @vlashada on 2025-09-10 01:07_

---

_Comment by @carljm on 2025-09-10 01:11_

Thanks for the report! This is #157 

---

_Closed by @carljm on 2025-09-10 01:11_

---
