```yaml
number: 637
title: False Negative when Enforcing Positional Arguments in regards to Protocols
type: issue
state: closed
author: max-muoto
labels:
  - Protocols
assignees: []
created_at: 2025-06-11T19:29:45Z
updated_at: 2025-09-12T10:10:32Z
url: https://github.com/astral-sh/ty/issues/637
synced_at: 2026-01-10T02:06:24Z
```

# False Negative when Enforcing Positional Arguments in regards to Protocols

---

_Issue opened by @max-muoto on 2025-06-11 19:29_

### Summary

Wasn't able to find this, but apologies if it's already been brought up in another issue, but this is more or less a rehash of https://github.com/python/mypy/issues/17421 for Ty.

In effect Ty doesn't raise a warning if you pass in an class implementation that enforces positional arguments on a method, for a protocol that does not.

Take this example, which should raise a warning but doesn't:

```python
from typing import Protocol


class Readable(Protocol):
    def read(self, n: int = -1) -> bytes: ...

class ActualReadable:
    def read(self, n: int = -1, /) -> bytes:
        return b""


def foo(readable: Readable) -> None:
    readable.read(n=1)


foo(ActualReadable())
```

This can lead to a runtime error if the protocol implementation is called using a key-word argument.

[Ty Playground](https://play.ty.dev/9793a133-5079-4305-9db6-a9c915b7cc50)

If this is simply the result of support for protocols not being finalized, feel free to close this.

---

_Label `Protocols` added by @carljm on 2025-06-12 05:14_

---

_Comment by @carljm on 2025-06-12 05:15_

I think our subtyping implementation for callable types already handles this correctly, so it should fall out correctly when we add support for validation of member types of protocols. But I'll let @AlexWaygood decide whether it's useful to keep this open separately or not.

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-08-22 13:10_

---

_Closed by @AlexWaygood on 2025-09-12 10:10_

---
