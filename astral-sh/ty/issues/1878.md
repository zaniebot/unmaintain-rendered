```yaml
number: 1878
title: Emit diagnostic on issubclass calls against protocols with non-method members
type: issue
state: open
author: AlexWaygood
labels:
  - Protocols
assignees: []
created_at: 2025-12-13T13:09:19Z
updated_at: 2025-12-13T13:09:19Z
url: https://github.com/astral-sh/ty/issues/1878
synced_at: 2026-01-10T01:55:00Z
```

# Emit diagnostic on issubclass calls against protocols with non-method members

---

_Issue opened by @AlexWaygood on 2025-12-13 13:09_

```pycon
>>> from typing import Protocol, runtime_checkable
>>> @runtime_checkable
... class X(Protocol):
...     y: int
...     
>>> issubclass(int, X)
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/var/containers/Bundle/Application/55202DCD-E142-4072-8C28-8E3DDC7B66E2/Pythonista3.app/Frameworks/Py3Kit.framework/pylib/abc.py", line 124, in __subclasscheck__
    return _abc_subclasscheck(cls, subclass)
  File "/var/containers/Bundle/Application/55202DCD-E142-4072-8C28-8E3DDC7B66E2/Pythonista3.app/Frameworks/Py3Kit.framework/pylib/typing.py", line 1570, in _proto_hook
    raise TypeError("Protocols with non-method members"
TypeError: Protocols with non-method members don't support issubclass()
```

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-13 13:09_

---

_Label `Protocols` added by @AlexWaygood on 2025-12-13 13:09_

---
