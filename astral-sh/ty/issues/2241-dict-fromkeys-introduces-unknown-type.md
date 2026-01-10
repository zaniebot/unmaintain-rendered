```yaml
number: 2241
title: dict.fromkeys introduces Unknown type
type: issue
state: closed
author: TylerYep
labels: []
assignees: []
created_at: 2025-12-27T22:43:53Z
updated_at: 2025-12-29T19:44:12Z
url: https://github.com/astral-sh/ty/issues/2241
synced_at: 2026-01-10T01:56:41Z
```

# dict.fromkeys introduces Unknown type

---

_Issue opened by @TylerYep on 2025-12-27 22:43_

### Summary

Ruff autofixes the first comprehension to the second version using `dict.fromkeys`, but ty flags the new code as having a type error.

```py
from enum import Enum
from typing import FrozenSet

class Fruit(Enum):
    APPLE = "apple"
    BANANA = "banana"

examples: frozenset[Fruit] = frozenset([Fruit.APPLE, Fruit.BANANA])
# result: list[dict[Fruit, float]] = [
#     {role: 0.0 for role in examples} for _ in range(5)
# ]
result2: list[dict[str, float]] = [
    dict.fromkeys(examples, 0.0) for _ in range(5)
]
```

`uv run ty check`

https://play.ty.dev/abf9edf5-c495-49e5-ac22-bae5b0a6f759

### Version

ty 0.0.7 (cf82a04b5 2025-12-24)

---

_Comment by @MatthewMckee4 on 2025-12-27 23:04_

Using `StrEnum` instead of `Enum` removes all diagnostics.

https://play.ty.dev/477b71ef-024f-4468-9ad6-1faa78ba9a89

---

_Comment by @MichaReiser on 2025-12-29 10:56_

All other type checkers (pyright, pyrefly, and mypy) also report an error here. I understand that is because `Fruit` isn't a subclass of `str`, that's why `Fruit` isn't assignable to `str`.

We can demonstrate this using this simplified version:

```py
from enum import StrEnum, Enum
from typing import FrozenSet

class Fruit(Enum):
    APPLE = "apple"
    BANANA = "banana"

fruit: str = Fruit.APPLE
```

As @MatthewMckee4 suggested, you can use `StrEnum` if you want that the enum values are assignable to `str`.

---

_Closed by @MichaReiser on 2025-12-29 10:56_

---

_Comment by @carljm on 2025-12-29 19:42_

I tried this out in Ruff, and C420 autofixes from this:

```py
from enum import Enum


class Fruit(Enum):
    APPLE = "apple"
    BANANA = "banana"


examples: frozenset[Fruit] = frozenset([Fruit.APPLE, Fruit.BANANA])
result: list[dict[Fruit, float]] = [
    {role: 0.0 for role in examples} for _ in range(5)
]
```

to this:

```py
from enum import Enum


class Fruit(Enum):
    APPLE = "apple"
    BANANA = "banana"


examples: frozenset[Fruit] = frozenset([Fruit.APPLE, Fruit.BANANA])
result: list[dict[Fruit, float]] = [
    dict.fromkeys(examples, 0.0) for _ in range(5)
]
```

And the latter version is fine in both ruff and ty. The problem here was changing the key type from `Fruit` to `str`, which is not a change Ruff would make, as far as I know.

---
