```yaml
number: 14206
title: Explicitly exported methods from private modules are not private
type: issue
state: open
author: gaborbernat
labels:
  - rule
  - docstring
assignees: []
created_at: 2024-11-08T15:56:35Z
updated_at: 2024-11-09T02:49:26Z
url: https://github.com/astral-sh/ruff/issues/14206
synced_at: 2026-01-12T15:54:53Z
```

# Explicitly exported methods from private modules are not private

---

_@gaborbernat_

With `ruff` private modules public methods is not considered public? (when it comes to the `D` rules?)

aka:
`src/module/db/objects.py:1:1: D100 Missing docstring in public module`
but:
`src/module/db/_objects.py:1:1`
shows no errors :thinking: which for module docstring is alright but methods that are public at very list should have docs... This follows the usual pattern of, then doing in `src/module/db/__init__.py`:

```
from ._objects import do

__all__ = ["do"]
```

I argue that in a private module stuff in `__all__` should be always considered public, because the user is explicitly enumerating that hey these are exported. And that logic works without multifile analysis.

---

_Label `rule` added by @charliermarsh on 2024-11-09 02:11_

---

_Label `docstring` added by @charliermarsh on 2024-11-09 02:11_

---

_Comment by @charliermarsh on 2024-11-09 02:12_

If I recall correctly, this was to match the `pydocstyle` behavior, though I'm not strongly opposed to changing it.

---

_Comment by @charliermarsh on 2024-11-09 02:49_

Also reported in https://github.com/astral-sh/ruff/issues/14219.

---
