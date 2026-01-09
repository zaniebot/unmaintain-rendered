---
number: 4462
title: Support for isorts add import functionality
type: issue
state: closed
author: gaborbernat
labels:
  - isort
assignees: []
created_at: 2023-05-17T00:27:00Z
updated_at: 2023-05-17T00:35:59Z
url: https://github.com/astral-sh/ruff/issues/4462
synced_at: 2026-01-07T13:12:14-06:00
---

# Support for isorts add import functionality

---

_Issue opened by @gaborbernat on 2023-05-17 00:27_

https://pycqa.github.io/isort/docs/configuration/options.html#add-imports
example use case to add the import annotations:

```toml
[tool.isort]
known_first_party = ["helpers", "package_info"]
profile = "black"
add_imports = ["from __future__ import annotations"]
```

---

_Comment by @charliermarsh on 2023-05-17 00:31_

We do support this, but it's under a different name: [`required-imports`](https://beta.ruff.rs/docs/settings/#required-imports).

---

_Label `isort` added by @charliermarsh on 2023-05-17 00:35_

---

_Comment by @charliermarsh on 2023-05-17 00:35_

(Happy to re-open if this doesn't solve your problem :))

---

_Closed by @charliermarsh on 2023-05-17 00:35_

---
