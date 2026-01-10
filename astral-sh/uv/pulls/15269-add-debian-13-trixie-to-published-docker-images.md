```yaml
number: 15269
title: Add Debian 13 trixie to published Docker images
type: pull_request
state: merged
author: smeng9
labels:
  - enhancement
assignees: []
merged: true
base: main
head: trixie-docker-image
created_at: 2025-08-14T04:24:16Z
updated_at: 2025-08-17T00:30:34Z
url: https://github.com/astral-sh/uv/pull/15269
synced_at: 2026-01-10T06:44:33Z
```

# Add Debian 13 trixie to published Docker images

---

_Pull request opened by @smeng9 on 2025-08-14 04:24_

Closes https://github.com/astral-sh/uv/issues/15230

---

_@zanieb approved on 2025-08-14 04:29_

Thank you!

---

_Label `enhancement` added by @zanieb on 2025-08-14 04:32_

---

_Renamed from "Add Debian 13 trixie to docker image" to "Add Debian 13 trixie to published Docker images" by @zanieb on 2025-08-14 04:32_

---

_Assigned to @zanieb by @zanieb on 2025-08-14 13:13_

---

_Comment by @zanieb on 2025-08-14 13:28_

#15277 should resolve those OIDC errors.

---

_Merged by @zanieb on 2025-08-14 13:54_

---

_Closed by @zanieb on 2025-08-14 13:54_

---

_Comment by @samypr100 on 2025-08-17 00:30_

Note, two small entries were missed near the top 😄 

```
          - debian:trixie-slim,trixie-slim
          - buildpack-deps:trixie,trixie
```

We can promote these to default for `debian-slim`/`debian` in 0.9.0 later down the line via

```
          - debian:trixie-slim,trixie-slim,debian-slim
          - buildpack-deps:trixie,trixie,debian
```

---
