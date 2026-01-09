---
number: 13507
title: "Idea: warn when a re-exported name is imported"
type: issue
state: open
author: dimaqq
labels:
  - type-inference
assignees: []
created_at: 2024-09-25T01:23:16Z
updated_at: 2024-11-26T16:35:12Z
url: https://github.com/astral-sh/ruff/issues/13507
synced_at: 2026-01-07T13:12:15-06:00
---

# Idea: warn when a re-exported name is imported

---

_Issue opened by @dimaqq on 2024-09-25 01:23_

Consider a library like this:

```py
# lib/__init__.py
from .exceptions import LibError
from .client import Client

__all__ = [
    "Client",
    "LibError",
]

# lib/exceptions.py
class LibError(Exception): ...

# lib/client.py
from .exceptions import LibError

class Client:
    def some_method(self):
        if something.bad:
            raise LibError("thou shalt not pass")
```

Then the user code imports these names:

```py
from lib import LibError  # OK, explicitly re-exported

from lib.exceptions import LibError  # OK, that's where it's defined

from lib.client import LibError  # WARN: lib.exceptions.LibError is re-exported via lib.client

from lib.client import Client  # OK
```

The reason to warn is that lib.client can change, for example to `raise exception.MoreSpecificError` instead, which would not be a breaking change is MoreSpecificError inherits from LibError.

However, in that case, LibError may no longer be re-exported via lib.client.

Thus user code that depends on LibError name being present on lib.client is mistaken.

Ideas how this could be implemented:
1. get the `__qualname__` for the imported name
2. check if the import path is different from qualname
3. check if the imported name is also available at lib top level or
4. check if the imported name is available at some intermediate level between lib and qualname
5. suggest a more direct import

---

_Label `multifile-analysis` added by @MichaReiser on 2024-09-25 06:43_

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:32_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:33_

---

_Comment by @johnthagen on 2024-11-26 16:35_

Related to Mypy's `no_implicit_reexport` feature:

- https://github.com/python/mypy/issues/10198

---

_Referenced in [drhagen/parsita#118](../../drhagen/parsita/issues/118.md) on 2024-11-26 16:35_

---
