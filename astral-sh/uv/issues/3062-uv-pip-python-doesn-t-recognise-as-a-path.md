```yaml
number: 3062
title: "`uv pip --python` doesn't recognise / as a path separator on Windows"
type: issue
state: closed
author: pfmoore
labels:
  - bug
  - windows
assignees: []
created_at: 2024-04-16T14:49:06Z
updated_at: 2024-04-17T14:02:21Z
url: https://github.com/astral-sh/uv/issues/3062
synced_at: 2026-01-10T05:40:32Z
```

# `uv pip --python` doesn't recognise / as a path separator on Windows

---

_Issue opened by @pfmoore on 2024-04-16 14:49_

To demonstrate:

```
❯ uv venv a
Using Python 3.12.2 interpreter at: C:\Users\Gustav\AppData\Local\Programs\Python\Python312\python.exe
Creating virtualenv at: a
Activate with: overlay use a\Scripts\activate.nu
❯ uv pip list --python a/Scripts/python.exe
error: Failed to locate Python interpreter at `a/Scripts/python.exe`
❯ uv pip list --python a\Scripts\python.exe
```

Problem is here: https://github.com/astral-sh/uv/blob/main/crates/uv-interpreter/src/find_python.rs#L46

The line indicated only handles the "main" separator (`\` on Windows). But `/` is a perfectly valid path separator on Windows...

---

_Comment by @charliermarsh on 2024-04-16 14:50_

Thanks, will fix.

---

_Label `bug` added by @charliermarsh on 2024-04-16 14:50_

---

_Label `windows` added by @charliermarsh on 2024-04-16 14:50_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-16 14:52_

---

_Comment by @pfmoore on 2024-04-16 14:53_

A further issue caused by this is that if I chdir into the `Scripts` directory of a venv, and run `uv pip list --python python.exe` it will search for `python.exe` on `PATH` rather than using the venv. So, `uv pip list --python .\python.exe` gives a different answer than `uv pip list --python python.exe`, which is at best weird, and personally I'd consider it incorrect.

---

_Comment by @zanieb on 2024-04-16 15:00_

I believe that problem would be addressed by the design at #2386 but I'd have to double check. Agree it's weird..

---

_Comment by @charliermarsh on 2024-04-16 15:05_

@zanieb - My brief skim is that it's slightly separate and could even be changed / implemented before we do #2386 (e.g. via what's proposed at https://github.com/astral-sh/uv/issues/3060#issuecomment-2059301772).

---

_Unassigned @charliermarsh by @charliermarsh on 2024-04-16 15:05_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-16 15:18_

---

_Comment by @charliermarsh on 2024-04-16 15:19_

I'll fix this specific failing (the separator detection) and the rest let's track in https://github.com/astral-sh/uv/issues/2386#issuecomment-2059345905.

---

_Comment by @charliermarsh on 2024-04-17 14:02_

I believe this was also fixed by #3064.

---

_Closed by @charliermarsh on 2024-04-17 14:02_

---
