```yaml
number: 13661
title: Increase deadsnake timeout to 15min
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/15min-to-live-snake
created_at: 2025-05-26T13:51:28Z
updated_at: 2025-05-27T14:37:25Z
url: https://github.com/astral-sh/uv/pull/13661
synced_at: 2026-01-10T11:10:42Z
```

# Increase deadsnake timeout to 15min

---

_Pull request opened by @konstin on 2025-05-26 13:51_

https://github.com/astral-sh/uv/actions/runs/15254949524/job/42900366590

---

_Review requested from @zanieb by @konstin on 2025-05-26 13:51_

---

_Label `internal` added by @konstin on 2025-05-26 13:51_

---

_@konstin reviewed on 2025-05-26 13:52_

---

_Review comment by @konstin on `.github/workflows/ci.yml`:973 on 2025-05-26 13:52_

@zanieb Do we still need those or can we switch to pbs?

---

_Renamed from "Increase deadsnkae timeout to 15min" to "Increase deadsnake timeout to 15min" by @konstin on 2025-05-26 20:31_

---

_@zanieb reviewed on 2025-05-27 14:33_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:973 on 2025-05-27 14:33_

That's a good question. It _seems_ ideal to keep testing against a third-party distribution since they may package it differently. I wonder if we can just use GitHub's though?

---

_@zanieb approved on 2025-05-27 14:34_

---

_Merged by @zanieb on 2025-05-27 14:37_

---

_Closed by @zanieb on 2025-05-27 14:37_

---

_Branch deleted on 2025-05-27 14:37_

---

_@zanieb reviewed on 2025-05-27 14:37_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:973 on 2025-05-27 14:37_

Tracking in #13681

---
