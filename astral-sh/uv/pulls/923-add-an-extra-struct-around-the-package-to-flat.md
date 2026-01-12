```yaml
number: 923
title: Add an extra struct around the package-to-flat index map
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/flat-index
created_at: 2024-01-15T04:19:04Z
updated_at: 2024-01-15T14:48:12Z
url: https://github.com/astral-sh/uv/pull/923
synced_at: 2026-01-12T16:04:17Z
```

# Add an extra struct around the package-to-flat index map

---

_@charliermarsh_

## Summary

`FlatIndex` is now the thing that's keyed on `PackageName`, while `FlatDistributions` is what used to be called `FlatIndex` (a map from version to `PrioritizedDistribution`, for a single package). I find this a bit clearer, since we can also remove the `from_files` that doesn't return `Self`, which I had trouble following.

---

_Label `internal` added by @charliermarsh on 2024-01-15 04:19_

---

_Review requested from @konstin by @charliermarsh on 2024-01-15 04:19_

---

_@konstin approved on 2024-01-15 08:47_

---

_Merged by @charliermarsh on 2024-01-15 14:48_

---

_Closed by @charliermarsh on 2024-01-15 14:48_

---

_Branch deleted on 2024-01-15 14:48_

---
