```yaml
number: 11189
title: "Add support for respecting `VIRTUAL_ENV` in project commands via `--active`"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/active
created_at: 2025-02-03T18:52:37Z
updated_at: 2025-02-06T11:20:56Z
url: https://github.com/astral-sh/uv/pull/11189
synced_at: 2026-01-12T16:09:43Z
```

# Add support for respecting `VIRTUAL_ENV` in project commands via `--active`

---

_@zanieb_

I think `UV_PROJECT_ENVIRONMENT` is too complicated for use-cases where the user wants to sync to the active environment. I don't see a compelling reason not to make opt-in easier. I see a lot of questions about how to deal with this warning in the issue tracker, but it seems painful to collect them here for posterity.

A notable behavior here — we'll treat this as equivalent to `UV_PROJECT_ENVIRONMENT` so... if you point us to a valid virtual environment that needs to be recreated for some reason (e.g., new Python version request), we'll happily delete it and start over.

---

_Label `cli` added by @zanieb on 2025-02-03 18:52_

---

_Comment by @zanieb on 2025-02-03 18:56_

I'm also considering support for `UV_PROJECT_ENVIRONMENT=ACTIVE`, `UV_PROJECT_ENVIRONMENT=VIRTUAL_ENV`, or `UV_PROJECT_USE_ACTIVE`

---

_Marked ready for review by @zanieb on 2025-02-03 19:57_

---

_Review requested from @charliermarsh by @zanieb on 2025-02-03 19:57_

---

_Comment by @danielhollas on 2025-02-05 10:27_

Another advantage of a dedicated flag is discoverability via command help. I was not aware of UV_PROJECT_ENVIRONMENT until now. `uv sync --help | grep UV_PROJECT` returns nothing.

---

_Comment by @zanieb on 2025-02-05 15:17_

Well, `UV_PROJECT_ENVIRONMENT` was intended to unblock people in edge-cases, it wasn't intended for use in normal workflows.

---

_@charliermarsh approved on 2025-02-05 15:21_

---

_Comment by @charliermarsh on 2025-02-05 15:22_

I think we should add this to the docs too before shipping.

---

_Merged by @zanieb on 2025-02-05 16:12_

---

_Closed by @zanieb on 2025-02-05 16:12_

---

_Branch deleted on 2025-02-05 16:12_

---

_Comment by @zmeir on 2025-02-06 06:55_

Thank you for this @zanieb !

I've been hit by this "issue" several times. From what I've seen, this mostly happens (to me at least) when I'm working on a project with a virtual env created by PyCharm - it creates `venv` by default, but `UV_PROJECT_ENVIRONMENT` defaults to `.venv`. And so I get this warning, delete `.venv`, and run the command again with `UV_PROJECT_ENVIRONMENT=venv`.  
This will definitely help 😃 

One follow-up question though:  
Is it possible to set `--active` as the default via config? Alternatively, is it possible to change the default `UV_PROJECT_ENVIRONMENT` via config?

---
