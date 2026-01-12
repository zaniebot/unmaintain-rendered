```yaml
number: 14278
title: add more proper docker login if
type: pull_request
state: merged
author: Gankra
labels:
  - internal
assignees: []
merged: true
base: main
head: gankra/fixif
created_at: 2025-06-26T15:54:42Z
updated_at: 2025-06-26T16:05:47Z
url: https://github.com/astral-sh/uv/pull/14278
synced_at: 2026-01-12T16:11:07Z
```

# add more proper docker login if

---

_@Gankra_

_No description provided._

---

_Label `internal` added by @Gankra on 2025-06-26 15:56_

---

_@zanieb reviewed on 2025-06-26 16:00_

---

_Review comment by @zanieb on `.github/workflows/build-docker.yml`:57 on 2025-06-26 16:00_

Won't this still be `false` on workflow dispatch?

---

_@zanieb reviewed on 2025-06-26 16:00_

---

_Review comment by @zanieb on `.github/workflows/build-docker.yml`:57 on 2025-06-26 16:00_

Ah I see, it's handled based on DRY_RUN

---

_@zanieb approved on 2025-06-26 16:00_

---

_@Gankra reviewed on 2025-06-26 16:02_

---

_Review comment by @Gankra on `.github/workflows/build-docker.yml`:57 on 2025-06-26 16:02_

Yes, but we ignore this value in favor of hardcoded login=true when it's not a dry-run (in the case of a dry-run workflow-dispatch, well, the logins are just to avoid ratelimiting so it's fine?).

---

_@zanieb reviewed on 2025-06-26 16:03_

---

_Review comment by @zanieb on `.github/workflows/build-docker.yml`:57 on 2025-06-26 16:03_

It's a little goofy, but fine with me

---

_Merged by @Gankra on 2025-06-26 16:05_

---

_Closed by @Gankra on 2025-06-26 16:05_

---

_Branch deleted on 2025-06-26 16:05_

---
