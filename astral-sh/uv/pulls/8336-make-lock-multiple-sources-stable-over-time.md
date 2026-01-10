```yaml
number: 8336
title: "Make `lock_multiple_sources` stable over time"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/fix-main-exclude-newer
created_at: 2024-10-18T15:45:29Z
updated_at: 2024-10-18T15:51:51Z
url: https://github.com/astral-sh/uv/pull/8336
synced_at: 2026-01-10T12:54:07Z
```

# Make `lock_multiple_sources` stable over time

---

_Pull request opened by @konstin on 2024-10-18 15:45_

Tests are failing on main (https://github.com/astral-sh/uv/actions/runs/11406444319/job/31740244721?pr=8320) due to an update to markupsafe. By switching from the torch index to testpypi which supports excludes-newer, we can make the test stable and fix main.

---

_Label `internal` added by @konstin on 2024-10-18 15:45_

---

_Review requested from @charliermarsh by @konstin on 2024-10-18 15:45_

---

_@charliermarsh approved on 2024-10-18 15:46_

Thank you!

---

_Merged by @konstin on 2024-10-18 15:51_

---

_Closed by @konstin on 2024-10-18 15:51_

---

_Branch deleted on 2024-10-18 15:51_

---
