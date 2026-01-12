```yaml
number: 15294
title: "`import-private-name (PLC2701)` does not respect `@type_check_only`"
type: issue
state: open
author: Avasam
labels:
  - wish
  - needs-decision
assignees: []
created_at: 2025-01-06T03:58:50Z
updated_at: 2025-01-06T08:05:49Z
url: https://github.com/astral-sh/ruff/issues/15294
synced_at: 2026-01-12T15:54:54Z
```

# `import-private-name (PLC2701)` does not respect `@type_check_only`

---

_@Avasam_

[import-private-name (PLC2701)](https://docs.astral.sh/ruff/rules/import-private-name/#import-private-name-plc2701) says that
> This rule ignores private name imports that are exclusively used in type annotations. Ideally, types would be public; however, this is not always possible when using third-party libraries.

However, it'll still trigger when the name is subclassed, but said class is decorated with `typing.type_check_only` in a stub file:
https://play.ruff.rs/658cc2b7-e7e0-4195-a19c-e1239a465e71
```py
from _typeshed import SupportsWrite
from typing import Protocol, TypeVar, type_check_only
_T_contra = TypeVar("_T_contra", contravariant=True)

@type_check_only
class _Stream(SupportsWrite[_T_contra], Protocol):
    def seek(self, offset: int, whence: int = ..., /) -> int: ...
```

---

_Comment by @MichaReiser on 2025-01-06 08:05_

Ruff doesn't support the `@type_check_only` decorator yet. So a first step would be to add support for it and then go through all the rules that need changing.

---

_Label `wish` added by @MichaReiser on 2025-01-06 08:05_

---

_Label `needs-decision` added by @MichaReiser on 2025-01-06 08:05_

---
