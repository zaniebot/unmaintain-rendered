---
number: 16213
title: Publish Standard Slim Docker Image
type: issue
state: open
author: rbebb
labels:
  - enhancement
assignees: []
created_at: 2025-10-09T18:57:51Z
updated_at: 2025-10-09T19:22:09Z
url: https://github.com/astral-sh/uv/issues/16213
synced_at: 2026-01-10T01:26:04Z
---

# Publish Standard Slim Docker Image

---

_Issue opened by @rbebb on 2025-10-09 18:57_

### Summary

I see that there are slim Docker images for each version of Debian, but it would be great to have a slim image that always uses the latest version of Debian (like python:3.13-slim).

### Example

`ghcr.io/astral-sh/uv:0.9-python3.13-slim`

---

_Label `enhancement` added by @rbebb on 2025-10-09 18:57_

---

_Comment by @zanieb on 2025-10-09 19:22_

Like `uv:debian-slim`? That exists. I don't think we'll make an additional one for each Python version.

---

_Referenced in [astral-sh/uv#16652](../../astral-sh/uv/issues/16652.md) on 2025-11-09 14:01_

---
