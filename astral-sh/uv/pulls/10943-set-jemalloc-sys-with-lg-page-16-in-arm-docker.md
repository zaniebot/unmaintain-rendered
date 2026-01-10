```yaml
number: 10943
title: "Set `JEMALLOC_SYS_WITH_LG_PAGE=16` in arm Docker builds"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/docker-jemalloc
created_at: 2025-01-24T19:36:20Z
updated_at: 2025-01-28T17:35:56Z
url: https://github.com/astral-sh/uv/pull/10943
synced_at: 2026-01-10T11:45:19Z
```

# Set `JEMALLOC_SYS_WITH_LG_PAGE=16` in arm Docker builds

---

_Pull request opened by @zanieb on 2025-01-24 19:36_

We do this in our standard binary release pipeline, but not in our Docker images.

See https://github.com/astral-sh/uv/issues/10942

---

_Label `bug` added by @zanieb on 2025-01-24 19:36_

---

_Comment by @zanieb on 2025-01-24 19:48_

I don't know how to test this. Builds fine though.

---

_Comment by @zanieb on 2025-01-24 19:49_

And the image seems to work (e.g., with `uv version`) on my aarch64 machine

---

_Marked ready for review by @zanieb on 2025-01-24 19:49_

---

_@charliermarsh approved on 2025-01-25 02:56_

---

_@konstin approved on 2025-01-28 17:34_

---

_Merged by @zanieb on 2025-01-28 17:35_

---

_Closed by @zanieb on 2025-01-28 17:35_

---

_Branch deleted on 2025-01-28 17:35_

---
