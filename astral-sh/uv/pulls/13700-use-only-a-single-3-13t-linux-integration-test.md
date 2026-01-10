```yaml
number: 13700
title: Use only a single 3.13t Linux integration test
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/integration-test-3.13t
created_at: 2025-05-28T13:23:50Z
updated_at: 2025-05-28T14:15:04Z
url: https://github.com/astral-sh/uv/pull/13700
synced_at: 2026-01-10T11:10:42Z
```

# Use only a single 3.13t Linux integration test

---

_Pull request opened by @konstin on 2025-05-28 13:23_

Fixes https://github.com/astral-sh/uv/issues/13681

We're still using the apt repo in the deadsnakes test but this is one less location with the flaky apt script.

---

_Review requested from @zanieb by @konstin on 2025-05-28 13:23_

---

_Label `internal` added by @konstin on 2025-05-28 13:23_

---

_@zanieb approved on 2025-05-28 13:48_

---

_@zanieb requested changes on 2025-05-28 13:48_

Actually, we're already doing this?

https://github.com/astral-sh/uv/blob/a0b27c7cff72ca7d10cb07454c1d480eed9f4b65/.github/workflows/ci.yml#L1352-L1360

---

_Renamed from "Install 3.13t from GitHub actions for integration test" to "Use only a single 3.13t Linux integration test" by @konstin on 2025-05-28 14:03_

---

_Comment by @konstin on 2025-05-28 14:03_

Do we need both?

---

_@zanieb approved on 2025-05-28 14:13_

---

_Comment by @zanieb on 2025-05-28 14:13_

Eh

---

_Merged by @konstin on 2025-05-28 14:15_

---

_Closed by @konstin on 2025-05-28 14:15_

---

_Branch deleted on 2025-05-28 14:15_

---
