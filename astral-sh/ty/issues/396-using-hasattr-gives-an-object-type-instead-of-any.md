```yaml
number: 396
title: Using hasattr gives an object type instead of Any
type: issue
state: closed
author: skraeven
labels: []
assignees: []
created_at: 2025-05-14T20:30:49Z
updated_at: 2025-05-15T00:42:20Z
url: https://github.com/astral-sh/ty/issues/396
synced_at: 2026-01-12T15:54:23Z
```

# Using hasattr gives an object type instead of Any

---

_@skraeven_

### Summary

When using hasattr on an unknown attribute, mypy will consider it an Any type, while ty considers it an object. This can give some errors when trying to use the attribute. Not sure if this is intended or a bug.

Example:
```
from typing import reveal_type


class Foo:
    pass


foo = Foo()

if hasattr(foo, "bar"):
    reveal_type(foo.bar)
    print(foo.bar.upper())

```

Gives:
```
Revealed type: `object` (revealed-type) [Ln 11, Col 17]
Type `object` has no attribute `upper` (unresolved-attribute) [Ln 12, Col 11]
```

https://play.ty.dev/c04860e6-d8fe-43d8-8f4a-a430e9e64bf2


### Version

_No response_

---

_Comment by @carljm on 2025-05-15 00:42_

This is an intentional choice. It's not clear why an `Any` type should be introduced here, giving up type checking, simply because the attribute's presence has been checked with `hasattr`. All we know from the `hasattr` check is that the attribute exists, and it must be some kind of Python object. From there further checks (e.g. `isinstance`) could be used to narrow the type of the attribute.

We can consider changing this if there's a strong case to do so, but for now I think I'll close as "not planned".

---

_Closed by @carljm on 2025-05-15 00:42_

---
