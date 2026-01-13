```yaml
number: 2481
title: "Invalid subscript assignment with key of type `Self@__get__` and value of type `Data` on object of type `dict[<special-form 'typing.Self'>, Data]`"
type: issue
state: closed
author: henriquegemignani
labels: []
assignees: []
created_at: 2026-01-13T15:45:03Z
updated_at: 2026-01-13T15:48:50Z
url: https://github.com/astral-sh/ty/issues/2481
synced_at: 2026-01-13T16:27:19Z
```

# Invalid subscript assignment with key of type `Self@__get__` and value of type `Data` on object of type `dict[<special-form 'typing.Self'>, Data]`

---

_@henriquegemignani_

### Summary

```python
from typing import Self, final

class Data:
    pass


@final
class RdvSignal:
    _map: dict[Self, Data] = {}

    def __get__(self) -> Data:
        tmp = self._map[self] = Data()
        return tmp

```
`Invalid subscript assignment with key of type `Self@__get__` and value of type `Data` on object of type `dict[<special-form 'typing.Self'>, Data]` `

https://play.ty.dev/87093785-711d-41d4-b473-edeb1ea23fee

With `mypy`, it gives an error if I remove the `@final` decorator. With it, everything is fine.

This is fairly similar to other issues related to Self, but the `@final` part feels different.

### Version

ty 0.0.11 (830cb9cc6 2026-01-09)

---

_Renamed from "Expected key of type `<special-form 'typing.Self'>`, got `Self@__get__`" to "Invalid subscript assignment with key of type `Self@__get__` and value of type `Data` on object of type `dict[<special-form 'typing.Self'>, Data]`" by @henriquegemignani on 2026-01-13 15:47_

---

_Comment by @AlexWaygood on 2026-01-13 15:48_

Thanks, this is #1124

---

_Closed by @AlexWaygood on 2026-01-13 15:48_

---
