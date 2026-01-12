```yaml
number: 1088
title: "Consolidate `UnusableDependencies` into a generic `Unavailable` incompatibility"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - error messages
assignees: []
merged: true
base: main
head: zb/unavailable
created_at: 2024-01-25T01:42:20Z
updated_at: 2024-01-25T05:39:56Z
url: https://github.com/astral-sh/uv/pull/1088
synced_at: 2026-01-12T16:04:25Z
```

# Consolidate `UnusableDependencies` into a generic `Unavailable` incompatibility

---

_@zanieb_

Requires https://github.com/zanieb/pubgrub/pull/20

In short, `UnusableDependencies` can be generalized into `Unavailable` which encompasses incompatibilities where a package range which is unusable for some inherent reason as well as when its dependencies are unusable. We can eventually use this to track more incompatibilities in the solver. I made the reason string required because I can't see a case where we should leave it out.

Additionally, this improves the display of conflicts in the root requirements.

---

_Review requested from @charliermarsh by @zanieb on 2024-01-25 01:51_

---

_Label `internal` added by @zanieb on 2024-01-25 02:06_

---

_Label `error messages` added by @zanieb on 2024-01-25 02:07_

---

_@charliermarsh approved on 2024-01-25 02:12_

Excellent.

---

_Merged by @zanieb on 2024-01-25 04:10_

---

_Closed by @zanieb on 2024-01-25 04:10_

---

_Branch deleted on 2024-01-25 04:10_

---

_Comment by @zanieb on 2024-01-25 05:39_

Oops, @charliermarsh / @konstin can one one of you review the PubGrub pull request too and I'll merge and follow up with an update to the pin here?

---

_Comment by @charliermarsh on 2024-01-25 05:39_

Yeah

---
