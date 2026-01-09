---
number: 284
title: Throw an error if a requirement exists with conflicting URLs
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-11-02T03:07:24Z
updated_at: 2024-08-13T14:14:16Z
url: https://github.com/astral-sh/uv/issues/284
synced_at: 2026-01-07T13:12:16-06:00
---

# Throw an error if a requirement exists with conflicting URLs

---

_Issue opened by @charliermarsh on 2023-11-02 03:07_

E.g., if you depend on Flask at one URL, and one of your dependencies depends on Flask at a different URL, we need to throw an error.

---

_Label `bug` added by @charliermarsh on 2023-11-02 03:18_

---

_Added to milestone `Feature complete` by @charliermarsh on 2023-11-02 03:19_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-02 12:56_

---

_Referenced in [astral-sh/uv#319](../../astral-sh/uv/pulls/319.md) on 2023-11-04 19:31_

---

_Closed by @charliermarsh on 2023-11-05 17:10_

---

_Referenced in [astral-sh/uv#424](../../astral-sh/uv/pulls/424.md) on 2023-11-14 16:52_

---

_Comment by @ayush-rathore-quartic on 2024-08-13 13:20_

Does anyone know a way to explicitly prefer one of the conflicting URLs or work around this error without updating the dependent packages? cc @charliermarsh 

---

_Comment by @charliermarsh on 2024-08-13 13:37_

@ayush-rathore-quartic -- You can use `--override overrides.txt`, and put something like:
```
flask @ git+https://github.com/pallets/flask@commit-sha
```

---

_Comment by @ayush-rathore-quartic on 2024-08-13 14:14_

@charliermarsh Thanks a whole lot for the prompt help, This worked!

---
