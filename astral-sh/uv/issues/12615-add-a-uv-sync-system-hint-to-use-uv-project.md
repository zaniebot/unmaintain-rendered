---
number: 12615
title: "Add a `uv sync --system` hint to use `UV_PROJECT_ENVIRONMENT`"
type: issue
state: open
author: zanieb
labels:
  - error messages
  - needs-decision
assignees: []
created_at: 2025-04-02T00:04:17Z
updated_at: 2025-04-02T00:05:43Z
url: https://github.com/astral-sh/uv/issues/12615
synced_at: 2026-01-10T01:25:22Z
---

# Add a `uv sync --system` hint to use `UV_PROJECT_ENVIRONMENT`

---

_Issue opened by @zanieb on 2025-04-02 00:04_

I'm a little hesitant, but... we could address confusion like https://github.com/astral-sh/uv/issues/12554 by adding a dummy flag that explains why this is not supported out of the box and what to do if you really want to do it?

---

_Label `error messages` added by @zanieb on 2025-04-02 00:04_

---

_Comment by @zanieb on 2025-04-02 00:05_

(We do already suggest `UV_PROJECT_ENVIRONMENT` in the Docker integration https://docs.astral.sh/uv/guides/integration/docker/#using-the-environment)

---

_Label `needs-decision` added by @zanieb on 2025-04-02 00:05_

---
