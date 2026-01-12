```yaml
number: 554
title: "Support detecting `Never` for exhausted union of literals"
type: issue
state: closed
author: Andrej730
labels: []
assignees: []
created_at: 2025-05-30T18:22:58Z
updated_at: 2025-05-30T18:42:52Z
url: https://github.com/astral-sh/ty/issues/554
synced_at: 2026-01-12T15:54:23Z
```

# Support detecting `Never` for exhausted union of literals

---

_@Andrej730_

### Summary

See the snippet below.

https://play.ty.dev/cfe44b44-5c65-4064-80be-ea24f7126e56

```python
from typing import Literal, assert_never

LType = Literal["A", "B"]

def test(l: LType):
    if l == "A":
        pass
    elif l == "B":
        pass
    else:
        # Revealed type: `@Todo & ~Literal["A"] & ~Literal["B"]` (revealed-type) [Ln 11, Col 21]
        reveal_type(l)
        # Argument does not have asserted type `Never` (type-assertion-failure) [Ln 12, Col 9]
        assert_never(l)
```

### Version

ty 0.0.1-alpha.7 (afb20f6fe 2025-05-26)

---

_Comment by @carljm on 2025-05-30 18:33_

Thanks for the report! We actually already support this, see https://play.ty.dev/70e3d9af-6362-4f04-ab56-d06394e46540

What's going wrong in your example is our inadequate handling of bare/implicit type aliases, which is #221 

It also works fine with a PEP 695 type alias: https://play.ty.dev/d380cf8f-2e3a-454c-aa98-8b337711907a

Closing this as duplicate of #221

---

_Closed by @carljm on 2025-05-30 18:34_

---

_Comment by @Andrej730 on 2025-05-30 18:39_

Thanks for pointing out, I've seen this issue in https://github.com/astral-sh/ty/issues/445, but for some reason didn't connected it to this, thinking it was about type aliases defined with `TypeAlias` explicitly 😅

---

_Comment by @carljm on 2025-05-30 18:42_

We are currently lacking support for both kinds of legacy type aliases, those using `TypeAlias` and those with no annotation. Hopefully will be fixed before long!

---
