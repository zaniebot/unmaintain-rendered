```yaml
number: 2328
title: "Skip INP001 for folders containing only *.pyi files"
type: issue
state: closed
author: twoertwein
labels:
  - bug
assignees: []
created_at: 2023-01-29T22:33:22Z
updated_at: 2023-01-31T22:35:32Z
url: https://github.com/astral-sh/ruff/issues/2328
synced_at: 2026-01-10T11:09:45Z
```

# Skip INP001 for folders containing only *.pyi files

---

_Issue opened by @twoertwein on 2023-01-29 22:33_

Folders containing only type stubs should not need `__init__.py` (not sure: but they might need an `__init__.pyi` )

---

_Label `question` added by @charliermarsh on 2023-01-29 22:38_

---

_Comment by @charliermarsh on 2023-01-29 22:38_

Ah true -- I'm not 100% sure either if they need `__init__.pyi`.

---

_Comment by @charliermarsh on 2023-01-29 22:38_

I think they _don't_, but haven't checked.

---

_Label `question` removed by @charliermarsh on 2023-01-31 21:34_

---

_Label `bug` added by @charliermarsh on 2023-01-31 21:34_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-31 21:34_

---

_Closed by @charliermarsh on 2023-01-31 22:35_

---
