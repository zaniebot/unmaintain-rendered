```yaml
number: 1151
title: Goto definition on operators
type: issue
state: closed
author: sharkdp
labels:
  - server
assignees: []
created_at: 2025-09-08T08:49:05Z
updated_at: 2025-10-23T13:42:30Z
url: https://github.com/astral-sh/ty/issues/1151
synced_at: 2026-01-10T02:06:25Z
```

# Goto definition on operators

---

_Issue opened by @sharkdp on 2025-09-08 08:49_

It would be great if I could go to the definition of unary and binary operators by clicking on `~` and `@` in this snippet, for example:

https://play.ty.dev/ff786392-5dc1-439e-9f37-c990a5f4b1e7
```py
from __future__ import annotations

class C:
    def __invert__(self) -> C:
        return C()

    def __matmul__(self, other) -> C:
        return C()

~C()

C() @ C()
```

---

_Label `server` added by @sharkdp on 2025-09-08 08:49_

---

_Assigned to @MichaReiser by @MichaReiser on 2025-10-20 14:02_

---

_Closed by @MichaReiser on 2025-10-21 17:25_

---

_Comment by @AlexWaygood on 2025-10-22 07:19_

Should this be reopened until we support goto on augmented assignment operators too?

---

_Comment by @MichaReiser on 2025-10-22 08:27_

> Should this be reopened until we support goto on augmented assignment operators too?

Do we want to support go to on augmented assignments? Pylance doesn't support it as far as I can tell and it's less clear if jumping to e.g. `__bool__` for `&=` seems is the correct behavior as it is a combined operator (it's just that there's no where to jump to for `=`)

---

_Comment by @AlexWaygood on 2025-10-23 13:42_

I don't know why pylance doesn't support it -- is it possible that that's just an oversight? To me it seems natural that we'd jump to `__iadd__` for `+=` (or `__add__`/`__radd__` if there's no `__iadd__` method and the operation falls back to one of those).

> it's less clear if jumping to e.g. `__bool__` for `&=` seems is the correct behavior as it is a combined operator

Do you mean `__iand__`? Not sure why `__bool__` would be involved for `&=` :-)

---
