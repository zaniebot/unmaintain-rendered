```yaml
number: 4048
title: Store explicit project on workspace member
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/explicit-project-on-workspace-member
created_at: 2024-06-05T14:25:38Z
updated_at: 2024-06-05T16:48:20Z
url: https://github.com/astral-sh/uv/pull/4048
synced_at: 2026-01-12T16:06:00Z
```

# Store explicit project on workspace member

---

_@konstin_

We know that `[project]` must exist for each workspace member, so we can store it directly and avoid going through the `.and_then()` when we need to access it. This requires cloning the struct due to lack of self-referential structs. An alternative would taking the `Project` from `PyProjectToml` instead, but this could be confusing when passing the `PyProjectToml` around.

---

_Review requested from @charliermarsh by @konstin on 2024-06-05 16:20_

---

_@charliermarsh approved on 2024-06-05 16:25_

---

_Label `internal` added by @charliermarsh on 2024-06-05 16:25_

---

_Merged by @charliermarsh on 2024-06-05 16:48_

---

_Closed by @charliermarsh on 2024-06-05 16:48_

---

_Branch deleted on 2024-06-05 16:48_

---
