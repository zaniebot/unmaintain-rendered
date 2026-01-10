```yaml
number: 1034
title: class inheritance and model_dump with pydantic models
type: issue
state: closed
author: CharlesPerrotMinot
labels:
  - question
assignees: []
created_at: 2025-08-18T05:29:33Z
updated_at: 2025-08-18T14:59:59Z
url: https://github.com/astral-sh/ty/issues/1034
synced_at: 2026-01-10T02:06:24Z
```

# class inheritance and model_dump with pydantic models

---

_Issue opened by @CharlesPerrotMinot on 2025-08-18 05:29_

### Question

We're getting a missing-argument when populating a class inheriting a pydantic BaseModel, with a model_dump:
```
Checking ------------------------------------------------------------ 1/1 files
error[missing-argument]: No argument provided for required parameter `foo`
   |
11 | def create_event(event: Event) -> SpecialEvent:
12 |     return SpecialEvent(**event.model_dump())
   |            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
info: rule `missing-argument` is enabled by default
```

Code sample is:
```
from pydantic import BaseModel


class Event(BaseModel):
    foo: str


class SpecialEvent(Event): ...


def create_event(event: Event) -> SpecialEvent:
    return SpecialEvent(**event.model_dump())
```

Since the input is an event, it will have the `foo` attribute, and so doing `**event.model_dump()` will populate the attribute in the class `SpecialEvent`. It's probably not a bug, but it would be nice if ty could support this use case, where an inherited Model is populated using `**instance.model_dump()`, which is the most convenient way to populate a pydantic class from another, without having to do it attribute by attribure.

### Version

[0.0.1-alpha.18](https://github.com/astral-sh/ty/releases/tag/0.0.1-alpha.18)

---

_Label `question` added by @CharlesPerrotMinot on 2025-08-18 05:29_

---

_Comment by @sharkdp on 2025-08-18 08:42_

Thank you for reporting this. It looks like a duplicate of https://github.com/astral-sh/ty/issues/247

---

_Closed by @sharkdp on 2025-08-18 08:42_

---

_Comment by @CharlesPerrotMinot on 2025-08-18 14:59_

Sorry I missed that! Thanks!

---
