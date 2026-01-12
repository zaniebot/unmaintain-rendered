```yaml
number: 37
title: Enable binary builds
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/build-binaries
created_at: 2025-05-05T20:01:38Z
updated_at: 2025-07-08T10:39:09Z
url: https://github.com/astral-sh/ty/pull/37
synced_at: 2026-01-12T15:54:27Z
```

# Enable binary builds

---

_@zanieb_

_No description provided._

---

_Renamed from "Renable binary builds on push" to "Enable binary builds" by @zanieb on 2025-05-05 20:29_

---

_Marked ready for review by @zanieb on 2025-05-05 20:30_

---

_Review comment by @Gankra on `.github/workflows/build-binaries.yml`:27 on 2025-05-05 20:45_

curious, why did this change?

---

_Review comment by @Gankra on `.github/workflows/build-binaries.yml`:47 on 2025-05-05 20:45_

similarly

---

_@Gankra approved on 2025-05-05 20:46_

---

_@zanieb reviewed on 2025-05-05 20:47_

---

_Review comment by @zanieb on `.github/workflows/build-binaries.yml`:27 on 2025-05-05 20:47_

Because the repo is private and `actions/checkout` failed without it. It must not be needed for public repos.

---

_@zanieb reviewed on 2025-05-05 20:47_

---

_Review comment by @zanieb on `.github/workflows/build-binaries.yml`:47 on 2025-05-05 20:47_

This can be removed, this was an attempt to debug the above issue.

---

_Merged by @zanieb on 2025-05-05 21:00_

---

_Closed by @zanieb on 2025-05-05 21:00_

---

_Branch deleted on 2025-07-08 10:39_

---
