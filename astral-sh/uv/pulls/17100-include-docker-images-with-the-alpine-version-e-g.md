```yaml
number: 17100
title: "Include Docker images with the alpine version, e.g., `python3.x-alpine3.23`"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/docker-alpine
created_at: 2025-12-12T13:26:21Z
updated_at: 2025-12-16T11:52:42Z
url: https://github.com/astral-sh/uv/pull/17100
synced_at: 2026-01-12T16:12:36Z
```

# Include Docker images with the alpine version, e.g., `python3.x-alpine3.23`

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/17095

This also stabilizes the Alpine version for users that do not choose to pin it. We could add this to the build matrix separately to avoid that, but I think that's okay?

---

_Review requested from @samypr100 by @zanieb on 2025-12-12 13:31_

---

_@samypr100 reviewed on 2025-12-12 13:38_

---

_Review comment by @samypr100 on `.github/workflows/build-docker.yml`:193 on 2025-12-12 13:38_

```suggestion
          - python:3.14-alpine3.22,python3.14-alpine3.22,python3.14-alpine
          - python:3.13-alpine3.22,python3.13-alpine3.22,python3.13-alpine
          - python:3.12-alpine3.22,python3.12-alpine3.22,python3.12-alpine
          - python:3.11-alpine3.22,python3.11-alpine3.22,python3.11-alpine
          - python:3.10-alpine3.22,python3.10-alpine3.22,python3.10-alpine
          - python:3.9-alpine3.22,python3.9-alpine3.22,python3.9-alpine
          - python:3.8-alpine3.20,python3.8-alpine3.20,python3.8-alpine
```

I would match the version we publish under `alpine:3.22,alpine3.22,alpine`

---

_@samypr100 reviewed on 2025-12-12 13:38_

---

_Review comment by @samypr100 on `.github/workflows/build-docker.yml`:193 on 2025-12-12 13:38_

Note 3.8 stopped updating after 3.20 and 3.9 will have no further alpine updates (no 3.23)

---

_@zanieb reviewed on 2025-12-12 13:39_

---

_Review comment by @zanieb on `.github/workflows/build-docker.yml`:193 on 2025-12-12 13:39_

This would be breaking though, the upstream image is already 3.23 so that's what it is today

---

_Review comment by @zanieb on `.github/workflows/build-docker.yml`:193 on 2025-12-12 13:40_

(We could change 3.9 and 3.8, I didn't notice those were stale upstream now)

---

_@zanieb reviewed on 2025-12-12 13:40_

---

_@samypr100 reviewed on 2025-12-12 13:59_

---

_Review comment by @samypr100 on `.github/workflows/build-docker.yml`:193 on 2025-12-12 13:59_

Ah, yes. uv>=0.9.16 published 3.23 as a result where as uv<=0.9.15 has been 3.22.



---

_@zanieb reviewed on 2025-12-12 14:01_

---

_Review comment by @zanieb on `.github/workflows/build-docker.yml`:193 on 2025-12-12 14:01_

I agree we should match versions long-term though

---

_@samypr100 approved on 2025-12-12 14:11_

---

_Marked ready for review by @zanieb on 2025-12-12 14:15_

---

_Merged by @zanieb on 2025-12-12 14:17_

---

_Closed by @zanieb on 2025-12-12 14:17_

---

_Branch deleted on 2025-12-12 14:17_

---

_Label `enhancement` added by @konstin on 2025-12-16 11:52_

---
