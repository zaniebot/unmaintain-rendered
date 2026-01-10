```yaml
number: 1335
title: "support `Generic` bound evaluation on `typing.Self`"
type: issue
state: closed
author: CoderJoshDK
labels: []
assignees: []
created_at: 2025-10-10T14:35:13Z
updated_at: 2025-10-10T23:06:56Z
url: https://github.com/astral-sh/ty/issues/1335
synced_at: 2026-01-10T02:06:25Z
```

# support `Generic` bound evaluation on `typing.Self`

---

_Issue opened by @CoderJoshDK on 2025-10-10 14:35_

Given the following python code

```py
from typing import Generic, Self, TypeVar

class Base: ...

V = TypeVar("V", bound="Base")
# V = TypeVar("V", bound="Base", covariant=True) # also fails


class Item(Generic[V]): ...

class Fine(Base): ...

class Problem(Base):
    item: Item[Self]
    good: Item[Fine]

    def __init__(self) -> None:
        super().__init__()

        self.item = Item()
        self.good = Item()
```

`ty` reports:

```sh
> uvx ty check
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                                                                error[invalid-argument-type]: Argument to class `Item` is incorrect
  --> main.py:18:11
   |
17 | class Problem(Base):
18 |     item: Item[Self]
   |           ^^^^^^^^^^ Expected `Base`, found `typing.Self`
19 |     good: Item[Fine]
   |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

As you can see, with `good` // `Fine`, `ty` correctly can determine that `Fine` is bound to `Base` but it can not evaluate `Self` as being bound to `Base`

<details><summary>Version</summary>
<p>

```sh
> uvx ty --version
ty 0.0.1-alpha.22 (2f3190d09 2025-10-10)
```

</p>
</details> 


<sub>Potentially related (but inverted) is #1124.</sub>

---

_Renamed from "support `Generic` bound with `typing.Self`" to "support `Generic` bound evaluation on `typing.Self`" by @CoderJoshDK on 2025-10-10 14:37_

---

_Comment by @carljm on 2025-10-10 23:06_

Thanks for the report! Although the presentation is a bit different, the root cause here is exactly the same as #1124; we don't have any support yet for `Self` as an attribute annotation.

---

_Closed by @carljm on 2025-10-10 23:06_

---
