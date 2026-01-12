```yaml
number: 2043
title: "`<TypedDict> |= {}` fails with `unsupported-operator`"
type: issue
state: open
author: zanieb
labels:
  - bug
  - bidirectional inference
assignees: []
created_at: 2025-12-17T23:30:54Z
updated_at: 2026-01-09T05:29:22Z
url: https://github.com/astral-sh/ty/issues/2043
synced_at: 2026-01-12T15:54:26Z
```

# `<TypedDict> |= {}` fails with `unsupported-operator`

---

_@zanieb_

https://play.ty.dev/3c2f6220-f3ad-48ed-b960-d80d9d611de2

```python
from typing_extensions import TypedDict

class ModelSettings(TypedDict):
    pass

base = ModelSettings()
overrides = ModelSettings()
base |= overrides # ok
base |= {} # error
```

Briefly discussed in the [dev channel](https://discord.com/channels/1039017663004942429/1343690517745111081/1450991812138369168)

---

_Label `bug` added by @zanieb on 2025-12-17 23:31_

---

_Comment by @zanieb on 2025-12-17 23:31_

@carljm suggests

> the two things we might need are 
> 1) support for type context on arguments to implicit dunder calls
> 2) support for a bounded type variable (Self in this case) as type context

---

_Added to milestone `Stable` by @carljm on 2025-12-17 23:32_

---

_Label `bidirectional inference` added by @ibraheemdev on 2025-12-18 00:21_

---

_Comment by @ibraheemdev on 2025-12-18 02:27_

I don't think we actually propagate type context through augmented assignments at all.

---

_Comment by @carljm on 2025-12-18 02:29_

Right: the suggestion in point 1 is that we should; where augmented assignment is merely one form of syntactic sugar for an implicit dunder call, where we could propagate type context like we do for any other call. 

---

_Assigned to @ibraheemdev by @ibraheemdev on 2026-01-09 05:29_

---
