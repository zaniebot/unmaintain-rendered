```yaml
number: 1218
title: "`Protocol` not understood as an `ABCMeta`"
type: issue
state: closed
author: KotlinIsland
labels: []
assignees: []
created_at: 2025-09-20T07:44:33Z
updated_at: 2025-09-22T07:29:04Z
url: https://github.com/astral-sh/ty/issues/1218
synced_at: 2026-01-10T02:06:25Z
```

# `Protocol` not understood as an `ABCMeta`

---

_Issue opened by @KotlinIsland on 2025-09-20 07:44_

### Summary

```py
from abc import ABC, ABCMeta
from typing import Protocol

a: ABCMeta
a = Protocol  # Object of type `typing.Protocol` is not assignable to `ABCMeta`
a = ABC
```

# ty in real life
```py
class Interface(Protocol):
    def f(self): ...

class Data(Enum, Interface):  # runtime error
    a = 1
    def f(self): ...
```

### Version

_No response_

---

_Closed by @AlexWaygood on 2025-09-22 07:29_

---
