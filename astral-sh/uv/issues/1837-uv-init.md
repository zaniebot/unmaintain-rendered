---
number: 1837
title: uv init
type: issue
state: closed
author: dbrtly
labels:
  - duplicate
  - projects
assignees: []
created_at: 2024-02-21T22:05:59Z
updated_at: 2024-02-22T15:01:45Z
url: https://github.com/astral-sh/uv/issues/1837
synced_at: 2026-01-10T01:23:09Z
---

# uv init

---

_Issue opened by @dbrtly on 2024-02-21 22:05_

Most of the time I do:

```
uv venv
source .venv/bin/activate || source .venv/Scripts/activate
uv pip install --upgrade pip
uv pip install -r requirements.txt

```

I'd like to add an alias `uv init` or `uv up` that does all that rather than copy/paste that boilerplate everywhere. Besides "use Makefile". 

---

_Label `project-management` added by @zanieb on 2024-02-22 15:01_

---

_Comment by @zanieb on 2024-02-22 15:01_

Thanks for the feedback. This is a duplicate of https://github.com/astral-sh/uv/issues/1360

---

_Closed by @zanieb on 2024-02-22 15:01_

---

_Label `duplicate` added by @zanieb on 2024-02-22 15:01_

---
