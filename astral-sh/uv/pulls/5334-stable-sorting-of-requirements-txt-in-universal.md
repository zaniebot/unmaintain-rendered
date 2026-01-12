```yaml
number: 5334
title: Stable sorting of requirements.txt in universal mode
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/stable-sorting-of-requirements-txt-in-univerval-mode
created_at: 2024-07-23T14:22:30Z
updated_at: 2024-07-23T14:46:34Z
url: https://github.com/astral-sh/uv/pull/5334
synced_at: 2026-01-12T16:06:45Z
```

# Stable sorting of requirements.txt in universal mode

---

_@konstin_

The `RequirementsTxtComparator` was written assuming there is one distribution per package name. This changed with the universal resolution, which allows multiple versions or urls for the same package name. The sorting we emitted for these new entries was incidental.

With this change, we properly sort these entries by name, version and then url in universal mode.

This is an output format change for `--universal` users.

---

_Label `enhancement` added by @konstin on 2024-07-23 14:22_

---

_Review requested from @charliermarsh by @konstin on 2024-07-23 14:41_

---

_@charliermarsh approved on 2024-07-23 14:45_

---

_Comment by @konstin on 2024-07-23 14:46_

Not sure what's going on with those two CI jobs, but they are not related to changes in this PR

---

_Merged by @konstin on 2024-07-23 14:46_

---

_Closed by @konstin on 2024-07-23 14:46_

---

_Branch deleted on 2024-07-23 14:46_

---
