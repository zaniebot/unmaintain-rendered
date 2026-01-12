```yaml
number: 8699
title: Publish Docker images to Docker Hub
type: issue
state: closed
author: gaby
labels:
  - wish
assignees: []
created_at: 2024-10-30T13:45:29Z
updated_at: 2024-10-30T14:47:17Z
url: https://github.com/astral-sh/uv/issues/8699
synced_at: 2026-01-12T15:59:32Z
```

# Publish Docker images to Docker Hub

---

_@gaby_

When using a Docker Registry to reduce network i/o and bandwidth costs you can't mirror images from `ghcr.io` as stated here: https://docs.docker.com/docker-hub/mirror/#gotcha

It would be great if the Docker images for `uv` were published to Docker Hub too.

---

_Comment by @zanieb on 2024-10-30 13:56_

I don't think we'll do this — I'm not comfortable spending time maintaining the images across multiple registries.

---

_Label `wish` added by @zanieb on 2024-10-30 13:56_

---

_Comment by @gaby on 2024-10-30 14:47_

Seems reasonable, thanks!

---

_Closed by @gaby on 2024-10-30 14:47_

---
