```yaml
number: 645
title: Implement name mangling semantics
type: issue
state: open
author: pratt-fds
labels:
  - runtime semantics
assignees: []
created_at: 2025-06-13T09:34:18Z
updated_at: 2025-11-18T16:10:32Z
url: https://github.com/astral-sh/ty/issues/645
synced_at: 2026-01-10T01:58:59Z
```

# Implement name mangling semantics

---

_Issue opened by @pratt-fds on 2025-06-13 09:34_

### Summary

Dunder private methods are not recognised. These methods have their names mangled on import (eg, Foo.__my_private_method becomes Foo._Foo__my_private_method), resulting in an unresolved-attribute error when validating test code

![Image](https://github.com/user-attachments/assets/183640c8-e876-4f61-9ad4-badbb33d2abe)

### Version

0.0.1a8

---

_Label `runtime semantics` added by @AlexWaygood on 2025-06-13 11:00_

---

_Renamed from "Dunder private methods not recognised" to "Implement name mangling" by @AlexWaygood on 2025-06-13 11:00_

---

_Comment by @AlexWaygood on 2025-06-13 11:01_

Thanks for opening the issue! I don't think we have anything tracking this yet.

For anybody else reading this: see https://docs.python.org/3/reference/expressions.html#private-name-mangling and https://docs.python.org/3/tutorial/classes.html#private-variables for the spec in Python's docs.

---

_Renamed from "Implement name mangling" to "Implement name mangling semantics" by @AlexWaygood on 2025-06-13 11:01_

---

_Comment by @carljm on 2025-11-14 15:00_

AFAICT no other type checker implements these semantics. I think we probably should at some point -- in general I prefer to model runtime accurately. But I think it is low priority.

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 15:00_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
