```yaml
number: 439
title: logging.StreamHandler(sys.stderr) triggers false no-matching-overload error in Pyright/ty
type: issue
state: closed
author: Dheebz
labels:
  - bug
  - generics
assignees: []
created_at: 2025-05-18T10:51:30Z
updated_at: 2025-05-18T12:08:06Z
url: https://github.com/astral-sh/ty/issues/439
synced_at: 2026-01-12T15:54:23Z
```

# logging.StreamHandler(sys.stderr) triggers false no-matching-overload error in Pyright/ty

---

_@Dheebz_

### Summary

Using StreamHandler(sys.stderr) or StreamHandler(sys.stdout) — both valid runtime calls — fails static type checking in ty (backed by Pyright). This appears to be due to overly strict or ambiguous overload resolution on StreamHandler.__init__.

### Behavior
```python
handler = logging.StreamHandler(sys.stderr)
```
Yields
```
error[no-matching-overload]: No overload of bound method `__init__` matches arguments
```

### Overloads Involved:
```python
@overload
def __init__(self: StreamHandler[TextIO], stream: None = None) -> None: ...
@overload
def __init__(self: StreamHandler[_StreamT], stream: _StreamT) -> None: ...
```
It seems that sys.stderr or sys.stdout (which are TextIO objects) fail to match either overload in Pyright's interpretation, especially when passed as a positional argument.

### Attempted Fixs

1. Explicity Cast:
```python
from typing import cast
handler = cast(logging.StreamHandler, logging.StreamHandler(sys.stderr))
```
2. Explicit Generic with TextIO:
```python
from typing import TextIO
handler: logging.StreamHandler[TextIO] = logging.StreamHandler(sys.stderr)
```
3. Default constructor + type annotations:
```python
handler: logging.StreamHandler = logging.StreamHandler()
```

### Minimal Reproducible Example
```python
import logging
import sys

handler = logging.StreamHandler(sys.stderr)
handler.setLevel(logging.INFO)
```

Note: Appologies for any spelling and/or if this issue requires additional information. 

### Version

ty 0.0.1-alpha.5

---

_Closed by @AlexWaygood on 2025-05-18 11:52_

---

_Comment by @AlexWaygood on 2025-05-18 11:56_

Thanks for the report! This issue is a duplicate of https://github.com/astral-sh/ty/issues/258, which in turn is likely caused by the same underlying problem as #370.

> fails static type checking in ty (backed by Pyright)

Just as an FYI — ty is a totally separate tool to pyright that makes many different decisions with regards to its behaviour. We don't depend on pyright in any way (and I'd be surprised if pyright issued the same false positive here, since it's a tool that's been around for a lot longer than we have!).

---

_Label `bug` added by @AlexWaygood on 2025-05-18 11:57_

---

_Label `generics` added by @AlexWaygood on 2025-05-18 11:57_

---

_Comment by @Dheebz on 2025-05-18 12:08_

Thank you @AlexWaygood for the prompt response and clarification, and marking as a duplicate. Appoligies for not seeing it before submitting it. 

I think this line through me which when thinking about it probably is coming from the typeshed/stdlib/logging.pyi.

```python
class StreamHandler(Handler, Generic[_StreamT]):
    stream: _StreamT  # undocumented
    terminator: str
    @overload
    def __init__(self: StreamHandler[TextIO], stream: None = None) -> None: ...
    @overload
    def __init__(self: StreamHandler[_StreamT], stream: _StreamT) -> None: ...  **# pyright: ignore[reportInvalidTypeVarUse]  #11780**
    def setStream(self, stream: _StreamT) -> _StreamT | None: ...
    if sys.version_info >= (3, 11):
        def __class_getitem__(cls, item: Any, /) -> GenericAlias: ...
```

In regards to pyright and false positives, i find ruff is significantly more reliable than pyright which is why i am so keen to give this a go. Cant wait to see how it all goes and the awesome tooling you will no doubt do in the future. 

---
