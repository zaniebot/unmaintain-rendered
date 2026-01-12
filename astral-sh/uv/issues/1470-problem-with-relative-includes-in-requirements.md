```yaml
number: 1470
title: problem with relative includes in requirements file
type: issue
state: closed
author: detlefla
labels:
  - duplicate
assignees: []
created_at: 2024-02-16T10:10:22Z
updated_at: 2024-02-16T14:55:42Z
url: https://github.com/astral-sh/uv/issues/1470
synced_at: 2026-01-12T15:58:28Z
```

# problem with relative includes in requirements file

---

_@detlefla_

Incompatibility: If pip-compile is invoked for a file requirements/dev.in and this file includes `-r common.in`, the included file will be searched in the same directory (requirements/) where the including file is located. `uv pip compile requirements/dev.in` obviously tries (and fails) to resolve the include from the caller's working directory.

---

_Comment by @hauntsaninja on 2024-02-16 10:57_

Duplicate of #1367 (and already fixed on main)

---

_Closed by @MichaReiser on 2024-02-16 12:14_

---

_Label `duplicate` added by @zanieb on 2024-02-16 14:55_

---
