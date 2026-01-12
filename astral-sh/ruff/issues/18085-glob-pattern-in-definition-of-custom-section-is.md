```yaml
number: 18085
title: Glob pattern in definition of custom section is not working
type: issue
state: closed
author: FateXRebirth
labels:
  - question
  - isort
assignees: []
created_at: 2025-05-14T06:24:45Z
updated_at: 2025-05-14T06:42:46Z
url: https://github.com/astral-sh/ruff/issues/18085
synced_at: 2026-01-12T15:54:56Z
```

# Glob pattern in definition of custom section is not working

---

_@FateXRebirth_

### Summary

According to https://docs.astral.sh/ruff/settings/#lint_isort_sections this doc, We are able to create a custom section and rearrange the order of sections, and I notice that you are using https://docs.rs/globset/latest/globset/ to handle that.

So I wrote the following code:
```toml
[lint.isort.sections]
"private" = ["[.]*"]

[lint.isort]
section-order = ["future", "standard-library", "third-party", "first-party", "local-folder", "private"]
```

What I want to achieve is just to place the package start with `.` or `..` to be placed in the end of all sections
Like this:
```python
import os
...
import pdfkit
...
from services.user_service import get_user
...
from .search_export import search_service
from ..search_export.service import get_result
```

But it is not working, I've tried the glob pattern in my rust project, it works as I expect

Is there anything I miss or I do wrong?

### Version

ruff 0.11.7

---

_Comment by @MichaReiser on 2025-05-14 06:31_

The glob match the module name, but the dots aren't part of the module name. This is because you can reference the same module by either using a relative import or an absolute import, but doing so shouldn't change into which section the module belongs (it's the same module after all). 


I skimmed through the settings and what you're looking for isn't supported by isort.


---

_Label `question` added by @MichaReiser on 2025-05-14 06:31_

---

_Label `isort` added by @MichaReiser on 2025-05-14 06:31_

---

_Closed by @FateXRebirth on 2025-05-14 06:42_

---
