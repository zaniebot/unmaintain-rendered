```yaml
number: 4141
title: "`uv lock` should not create a virtual environment"
type: issue
state: closed
author: zanieb
labels:
  - preview
assignees: []
created_at: 2024-06-07T19:12:31Z
updated_at: 2024-06-07T21:56:46Z
url: https://github.com/astral-sh/uv/issues/4141
synced_at: 2026-01-12T15:58:48Z
```

# `uv lock` should not create a virtual environment

---

_@zanieb_

`uv lock` creates a `.venv` directory in my working directory. I do not expect it to need to create a user-facing virtual environment to generate a lock file.

---

_Label `preview` added by @zanieb on 2024-06-07 19:12_

---

_Comment by @charliermarsh on 2024-06-07 19:14_

We do need a Python interpreter though, because we need to be able to build dependencies.

---

_Comment by @zanieb on 2024-06-07 19:20_

That's okay we discover the Python interpreter separately. Why do we need to create an environment with it? Don't we create temporary environments for builds?

---

_Comment by @charliermarsh on 2024-06-07 19:21_

Yes I’m not necessarily disagreeing.

---

_Assigned to @zanieb by @zanieb on 2024-06-07 20:54_

---

_Closed by @zanieb on 2024-06-07 21:56_

---
