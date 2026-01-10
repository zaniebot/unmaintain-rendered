---
number: 19559
title: "Rule suggestion: always import from definition"
type: issue
state: open
author: gorewilliams
labels:
  - rule
  - type-inference
  - needs-decision
assignees: []
created_at: 2025-07-25T15:09:20Z
updated_at: 2025-07-28T10:18:08Z
url: https://github.com/astral-sh/ruff/issues/19559
synced_at: 2026-01-10T01:23:00Z
---

# Rule suggestion: always import from definition

---

_Issue opened by @gorewilliams on 2025-07-25 15:09_

### Summary

When writing Python packages, I often export functions/classes in `__init__.py` files to make imports shorter and neater for package users. But when I work on the package itself, I'd like Ruff to stop me (the package developer) from using these shortcut imports, as they make the code confusing and may lead to unintended circular imports.

Here's a rough example (no pun intended):

`supercoolpkg/__init__.py`
```python3
from supercoolpkg.http_client import HTTPClient
from supercoolpkg.http_error import HTTPError
```

`supercoolpkg/http_error.py`
```python3
class HTTPError(Exception):
    pass
```

`supercoolpkg/http_client.py`
```python3
from supercoolpkg import HTTPError  # ERROR: Circular import


class HTTPClient:
     def get(self, url: str) -> str:
        try:
            ...
        except Exception as e:
           raise HTTPError(...) from e
```

Instead:
```python3
from supercoolpkg.http_error import HTTPError  # Imported from definition, no circular import


class HTTPClient:
     def get(self, url: str) -> str:
        try:
            ...
        except Exception as e:
           raise HTTPError(...) from e
```

A rule that enforces importing from definition (or alternatively disallows importing from `__init__.py`) will prevent this.

Would love to hear your opinion on this.
Many thx in advance!

---

_Comment by @ntBre on 2025-07-25 19:58_

This sounds similar and possibly overlaps with https://github.com/astral-sh/ruff/issues/167 and maybe https://github.com/astral-sh/ruff/issues/1107. I think it still needs the type-inference/multi-file resolution we're working on in ty.

---

_Label `rule` added by @ntBre on 2025-07-25 19:58_

---

_Label `needs-decision` added by @ntBre on 2025-07-25 19:58_

---

_Comment by @gorewilliams on 2025-07-25 20:07_

Thanks for the swift reply. Yeah makes sense that this would need ty. Exciting times :)

---

_Label `type-inference` added by @MichaReiser on 2025-07-28 10:18_

---

_Referenced in [astral-sh/ruff#20131](../../astral-sh/ruff/issues/20131.md) on 2025-08-28 13:08_

---

_Referenced in [astral-sh/ruff#20748](../../astral-sh/ruff/issues/20748.md) on 2025-10-07 16:11_

---

_Referenced in [astral-sh/ruff#21182](../../astral-sh/ruff/issues/21182.md) on 2025-11-03 18:36_

---
