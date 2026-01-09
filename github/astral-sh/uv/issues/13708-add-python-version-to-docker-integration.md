---
number: 13708
title: Add .python-version to docker integration
type: issue
state: open
author: mathijshenquet
labels:
  - documentation
assignees: []
created_at: 2025-05-28T20:33:19Z
updated_at: 2025-05-28T20:38:30Z
url: https://github.com/astral-sh/uv/issues/13708
synced_at: 2026-01-07T13:12:18-06:00
---

# Add .python-version to docker integration

---

_Issue opened by @mathijshenquet on 2025-05-28 20:33_

https://github.com/astral-sh/uv/blob/72e2821d26d52f97268d318c5c70de6b336c291f/docs/guides/integration/docker.md?plain=1#L375-L379

This should include .python-version bind mount

---

_Comment by @zanieb on 2025-05-28 20:37_

Seems reasonable to me.

---

_Comment by @zanieb on 2025-05-28 20:38_

Although, we're using `FROM python:3.12-slim` there? I'm not sure we want to override that?

---

_Label `documentation` added by @zanieb on 2025-05-28 20:38_

---
