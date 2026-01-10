```yaml
number: 1725
title: "`invalid-parameter-default` for `Type[T]`"
type: issue
state: closed
author: aidandj
labels: []
assignees: []
created_at: 2025-12-02T17:32:37Z
updated_at: 2025-12-02T17:35:29Z
url: https://github.com/astral-sh/ty/issues/1725
synced_at: 2026-01-10T01:56:40Z
```

# `invalid-parameter-default` for `Type[T]`

---

_Issue opened by @aidandj on 2025-12-02 17:32_

### Summary

TypeVar bound with a class. Function parameter with a default being that class. Passes pyright.

```
from typing import TypeVar, Type

class C: ...

T = TypeVar("T", bound=C)

def f(t: Type[T] = C): # Default value of type `<class 'C'>` is not assignable to annotated parameter type `type[T@f]` (invalid-parameter-default) [Ln 7, Col 7]
    pass
```

https://play.ty.dev/16be61a0-7bdf-4f8c-969b-4cd86fd394be

### Version

0.0.1alpha29

---

_Comment by @ibraheemdev on 2025-12-02 17:35_

Thanks for the report. This is https://github.com/astral-sh/ty/issues/592.

---

_Closed by @ibraheemdev on 2025-12-02 17:35_

---
