```yaml
number: 1959
title: "Autocomplete for PEP692 Unpack[TypedDict]"
type: issue
state: open
author: mflova
labels:
  - server
  - completions
assignees: []
created_at: 2025-12-16T20:36:15Z
updated_at: 2026-01-06T13:12:15Z
url: https://github.com/astral-sh/ty/issues/1959
synced_at: 2026-01-12T15:54:26Z
```

# Autocomplete for PEP692 Unpack[TypedDict]

---

_@mflova_

PEP 692 allows typing **kwargs using TypedDict and Unpack:

```py
from typing import TypedDict, Unpack

class Movie(TypedDict):
    name: str
    year: int

def foo(**kwargs: Unpack[Movie]) -> None: ... 
```

When calling `foo`, the LSP could detect that the allowed `kwargs` are those defined by `Movie` (name, year). If that's the case, autocompletion to `name=` and `year=` could be provided.

---

_Label `server` added by @AlexWaygood on 2025-12-16 20:37_

---

_Label `completions` added by @AlexWaygood on 2025-12-16 20:37_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-17 08:20_

---

_Comment by @eavanvalkenburg on 2026-01-06 13:12_

+1 for this, and Pylance does work with this!

---
