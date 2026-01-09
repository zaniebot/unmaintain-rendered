---
number: 10494
title: uv init creates the .venv outside of the project instead inside of the project folder.
type: issue
state: closed
author: v3ss0n
labels:
  - needs-mre
assignees: []
created_at: 2025-01-11T12:33:28Z
updated_at: 2025-01-25T02:51:53Z
url: https://github.com/astral-sh/uv/issues/10494
synced_at: 2026-01-07T13:12:18-06:00
---

# uv init creates the .venv outside of the project instead inside of the project folder.

---

_Issue opened by @v3ss0n on 2025-01-11 12:33_

When you do `uv init` and install packages , it creates the project folder but venv is outside of project folder. It was doing as expected before. Is that a bug?

---

_Comment by @charliermarsh on 2025-01-11 12:40_

I don’t believe anything changed here, but I’d need a minimal reproduction to help you (like a series of commands and a Git repository I can clone).

---

_Label `needs-mre` added by @charliermarsh on 2025-01-11 12:40_

---

_Comment by @charliermarsh on 2025-01-25 02:51_

Closing due to lack of follow-up.

---

_Closed by @charliermarsh on 2025-01-25 02:51_

---
