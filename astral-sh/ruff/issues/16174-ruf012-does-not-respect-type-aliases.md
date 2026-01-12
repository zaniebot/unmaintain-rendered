```yaml
number: 16174
title: RUF012 does not respect type aliases
type: issue
state: open
author: marcuslimdw
labels:
  - bug
  - type-inference
assignees: []
created_at: 2025-02-15T02:48:24Z
updated_at: 2025-02-17T09:40:25Z
url: https://github.com/astral-sh/ruff/issues/16174
synced_at: 2026-01-12T15:54:55Z
```

# RUF012 does not respect type aliases

---

_@marcuslimdw_

### Description

On 0.9.6, the following snippet triggers RUF012:

```
from collections.abc import Mapping
from typing import TypeAlias

Values: TypeAlias = Mapping[str, str]


class Base:
    values: Values


class Derived(Base):
    values: Values = {"a": "b"}  # RUF012 
```

However, if `values: Values` is replaced with `values: Mapping[str, str]`, the error goes away.

Using the Python 3.12 `type` statement instead of `TypeAlias` does not change the behaviour.

---

_Comment by @ntBre on 2025-02-15 03:11_

Thanks for the report! I think we could probably try to resolve the type alias here, at least within the same file.

The general case requires type inference as mentioned in some related issues like #13137, https://github.com/astral-sh/ruff/issues/13630, https://github.com/astral-sh/ruff/issues/5639, and https://github.com/astral-sh/ruff/issues/5429.

---

_Label `bug` added by @ntBre on 2025-02-15 03:11_

---

_Label `type-inference` added by @ntBre on 2025-02-15 03:11_

---

_Comment by @vladNed on 2025-02-17 09:34_

I can work on this if its fine with you guys. 

---

_Comment by @MichaReiser on 2025-02-17 09:40_

Sure, go for it

---

_Assigned to @vladNed by @MichaReiser on 2025-02-17 09:40_

---
