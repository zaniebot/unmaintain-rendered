---
number: 11317
title: "How to update `uv` managed Python version for applications?"
type: issue
state: closed
author: laenan8466
labels:
  - question
assignees: []
created_at: 2025-02-07T15:19:14Z
updated_at: 2025-02-07T15:35:05Z
url: https://github.com/astral-sh/uv/issues/11317
synced_at: 2026-01-07T13:12:18-06:00
---

# How to update `uv` managed Python version for applications?

---

_Issue opened by @laenan8466 on 2025-02-07 15:19_

### Question

I have Python installed via uv's `uv python install 3.x`. The project is an project/application (so not a library).

Let's say there is a new Python (standalone) release, e.g. 
3.12.9 -> 3.13.2
3.13.1 -> 3.13.2

It there a way to 'update' the standalone Python from uv, also replacing the existing `.venv`?

My current solution is to install the new python with `uv python install 3.x`, deleting the .venv and recreating it with the target python version (and delete the `.python-version` file). This works fine, but is a rather tedious process.
I would love to have a project scoped command like `uv sync --update-python-to 3.13.2`  (handling also the installation of the standalone-python).
Similar possibilities: 
- `uv python update` (either updating existing standalone python versions on patch level AND/OR updating project .venv on python version patch level)
- `uv python update 3.13` (updating project .venv to latest specified python version)

I guess there are smarter persons, having better ideas how to handle these commands. Is there something in uv right now, that I'm misssing?

### Platform

_No response_

### Version

0.5.29

---

_Label `question` added by @laenan8466 on 2025-02-07 15:19_

---

_Renamed from "How to update `uv` managed Python version for Application?" to "How to update `uv` managed Python version for applications?" by @laenan8466 on 2025-02-07 15:19_

---

_Comment by @zanieb on 2025-02-07 15:24_

We don't support automating this yet, but yeah we want to. It's tracked in https://github.com/astral-sh/uv/issues/7892 and some related issues are linked there.

---

_Closed by @zanieb on 2025-02-07 15:24_

---

_Comment by @laenan8466 on 2025-02-07 15:35_

Dang, didn't found that one. Thx!

---
