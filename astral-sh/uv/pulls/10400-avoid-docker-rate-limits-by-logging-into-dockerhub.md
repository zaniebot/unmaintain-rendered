```yaml
number: 10400
title: Avoid Docker rate limits by logging into DockerHub
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/docker-rate
created_at: 2025-01-08T17:05:30Z
updated_at: 2025-01-08T18:46:25Z
url: https://github.com/astral-sh/uv/pull/10400
synced_at: 2026-01-12T16:09:16Z
```

# Avoid Docker rate limits by logging into DockerHub

---

_@zanieb_

The latest release flaked failing to fetch the buildx image, which is reportedly due to rate limits. Last I checked, DockerHub enforces much stricter limits on unauthenticated requests. I added a bot account and a corresponding read-only token.

---

_Label `internal` added by @zanieb on 2025-01-08 17:05_

---

_Marked ready for review by @zanieb on 2025-01-08 17:07_

---

_@charliermarsh approved on 2025-01-08 18:16_

---

_@Gankra approved on 2025-01-08 18:16_

did you intentionally skip the docker-publish task on L108?

---

_Comment by @zanieb on 2025-01-08 18:22_

Nope! Good catch, thanks :)

---

_Merged by @zanieb on 2025-01-08 18:46_

---

_Closed by @zanieb on 2025-01-08 18:46_

---

_Branch deleted on 2025-01-08 18:46_

---
