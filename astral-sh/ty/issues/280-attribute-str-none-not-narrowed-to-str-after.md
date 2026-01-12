```yaml
number: 280
title: "Attribute `str | None` not narrowed to `str` after truthiness check"
type: issue
state: closed
author: blueraft
labels:
  - narrowing
assignees: []
created_at: 2025-05-08T17:29:09Z
updated_at: 2025-05-08T17:57:39Z
url: https://github.com/astral-sh/ty/issues/280
synced_at: 2026-01-12T15:54:22Z
```

# Attribute `str | None` not narrowed to `str` after truthiness check

---

_@blueraft_

### Summary

Sorry if this is a duplicate!

When an attribute `obj.attr` of type `str | None` is used in an if `obj.attr`: condition, its type is not narrowed to `str` within the if block (I assume this is true for any type).


```py
from typing import reveal_type
from dataclasses import dataclass

@dataclass
class Fruit:
    name: str | None

def some_fn(fruit: Fruit):
    if fruit.name:
        c = fruit.name
        reveal_type(c) # ty: str | None

```
https://play.ty.dev/0852e944-7f56-485d-835b-490ff67aeb4e

mypy and pyright both infer this as `str`.

https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMogCmAboQIYA2A%2BvAoQFCiRQAmZMZAxhWQM6%2BFembLnxsO3Pr3r0AAuK49%2B9SfygAxEAFckMAFz0oRqCjIRCeqLxggoAHygA5MCgb0WhYFciEqwFAAUoDr6Gtq6AJQGxphewboAdKbm0THGnFAAvFDxMElmDGnGRKSUNIiEAZwRQA


### Version

_No response_

---

_Comment by @MichaReiser on 2025-05-08 17:50_

I think this is https://github.com/astral-sh/ty/issues/164

---

_Label `narrowing` added by @MichaReiser on 2025-05-08 17:50_

---

_Comment by @blueraft on 2025-05-08 17:54_

Oh yes, I'll close this thanks!

---

_Closed by @blueraft on 2025-05-08 17:54_

---

_Closed by @MichaReiser on 2025-05-08 17:57_

---
