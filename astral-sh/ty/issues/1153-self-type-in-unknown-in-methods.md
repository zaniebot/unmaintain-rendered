```yaml
number: 1153
title: self type in Unknown in methods
type: issue
state: closed
author: LoicRiegel
labels: []
assignees: []
created_at: 2025-09-09T06:37:09Z
updated_at: 2025-09-09T06:48:35Z
url: https://github.com/astral-sh/ty/issues/1153
synced_at: 2026-01-12T15:54:24Z
```

# self type in Unknown in methods

---

_@LoicRiegel_

### Summary

Hi, I noticed that the ``self`` argument in any instance method of a class is unknown. This is not the case with mypy, which understands the correct type.

Simple example to reproduce:
```py
from typing import reveal_type


class SomeClass:
    def test_method_(self) -> None:
        reveal_type(self)  # ty: Unknown. Mypy: SomeClass
```

Expected: ty understands that ``self`` is of type ``SomeClass``.

### Version

ty 0.0.1-alpha.20 (f41f00af1 2025-09-03)

---

_Comment by @sharkdp on 2025-09-09 06:42_

Thank you for reporting this. We're working on this, see #159 

---

_Closed by @sharkdp on 2025-09-09 06:42_

---

_Comment by @LoicRiegel on 2025-09-09 06:48_

Oh that's great, I searched in the issues but didn't find this one!

---
