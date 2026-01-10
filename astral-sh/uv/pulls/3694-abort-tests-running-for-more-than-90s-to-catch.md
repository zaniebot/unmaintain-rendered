```yaml
number: 3694
title: Abort tests running for more than 90s to catch deadlocks
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/catch-deadlocks
created_at: 2024-05-21T12:08:48Z
updated_at: 2024-05-21T13:19:58Z
url: https://github.com/astral-sh/uv/pull/3694
synced_at: 2026-01-10T14:32:20Z
```

# Abort tests running for more than 90s to catch deadlocks

---

_Pull request opened by @konstin on 2024-05-21 12:08_

We observed recent deadlocks (e.g. https://github.com/astral-sh/uv/actions/runs/9173987312/job/25223866259?pr=3693) causing CI to run until the default github actions timeout. To prevent this, we add a 90s timeout in nextest. 90s should be enough both on CI and on developers' machines.

---

_Label `internal` added by @konstin on 2024-05-21 12:08_

---

_Review requested from @charliermarsh by @konstin on 2024-05-21 12:08_

---

_@charliermarsh reviewed on 2024-05-21 12:09_

---

_Review comment by @charliermarsh on `.config/nextest.toml`:5 on 2024-05-21 12:09_

Looks like a trailing blank line here.

---

_@charliermarsh reviewed on 2024-05-21 12:09_

---

_Review comment by @charliermarsh on `.config/nextest.toml`:4 on 2024-05-21 12:09_

Oh wow `terminate-after` is a multiple of `period`? Interesting.

---

_@charliermarsh approved on 2024-05-21 12:09_

Thx.

---

_Renamed from "Abort tests running for more than 60s to catch deadlocks" to "Abort tests running for more than 90s to catch deadlocks" by @konstin on 2024-05-21 12:50_

---

_Comment by @konstin on 2024-05-21 12:51_

Increasing to 90s due to macos

---

_Merged by @konstin on 2024-05-21 13:19_

---

_Closed by @konstin on 2024-05-21 13:19_

---

_Branch deleted on 2024-05-21 13:19_

---
