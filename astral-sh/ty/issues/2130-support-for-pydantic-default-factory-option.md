```yaml
number: 2130
title: "Support for Pydantic `default_factory` option inside `Annotated`"
type: issue
state: open
author: gusutabopb
labels:
  - library
assignees: []
created_at: 2025-12-20T15:12:36Z
updated_at: 2025-12-20T19:52:34Z
url: https://github.com/astral-sh/ty/issues/2130
synced_at: 2026-01-10T01:56:41Z
```

# Support for Pydantic `default_factory` option inside `Annotated`

---

_Issue opened by @gusutabopb on 2025-12-20 15:12_

### Summary

Related to https://github.com/pydantic/pydantic/issues/6713

Currently ty doesn't detect that a field is optional in a Pydantic model constructor when using the `Annotated[<type>, <annotations>]` syntax, which is currently recommended way to add `Field` constraints.

Are there plans to support this? If not is there some other (not Pydantic-specific) annotation that could be used to tell ty the given parameter is not mandatory?

### Sample code

```python
from typing import Annotated

from pydantic import BaseModel, Field


class Model(BaseModel):
    name: str
    options: Annotated[list[str], Field(default_factory=list)]

m = Model(name="John")
```


### Error

```
error[missing-argument]: No argument provided for required parameter `options`
  --> main.py:10:5
   |
 8 |     options: Annotated[list[str], Field(default_factory=list)]
 9 |
10 | m = Model(name="John")
   |     ^^^^^^^^^^^^^^^^^^
   |
info: rule `missing-argument` is enabled by default

Found 1 diagnostic
```

### Workaround

The code below works fine.

```python
from pydantic import BaseModel, Field


class Model(BaseModel):
    name: str
    options: list[str] = Field(default_factory=list)


m = Model(name="John")
```


PS: I couldn't past a playground link because I wasn't able to add third-party packages (i.e. Pydantic)

### Version

ty 0.0.4 (c1e6188b1 2025-12-18)

---

_Label `library` added by @AlexWaygood on 2025-12-20 15:16_

---

_Added to milestone `Stable` by @carljm on 2025-12-20 16:22_

---

_Renamed from "Support for Pydantic `default_factory` option inside `Annotated" to "Support for Pydantic `default_factory` option inside `Annotated`" by @gusutabopb on 2025-12-20 19:52_

---
