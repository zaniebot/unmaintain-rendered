```yaml
number: 3650
title: "Remove `SourceDistFilename` from `RegistrySourceDist`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/filename
created_at: 2024-05-18T16:56:25Z
updated_at: 2024-05-20T13:25:24Z
url: https://github.com/astral-sh/uv/pull/3650
synced_at: 2026-01-10T14:32:20Z
```

# Remove `SourceDistFilename` from `RegistrySourceDist`

---

_Pull request opened by @charliermarsh on 2024-05-18 16:56_

## Summary

Uncertain about this, but we don't actually need the full `SourceDistFilename`, only the name and version -- and we often have that information already (as in the lockfile routines). So by flattening the fields onto `RegistrySourceDist`, we can avoid re-parsing for information we already have.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-05-18 16:56_

---

_Label `internal` added by @charliermarsh on 2024-05-18 16:56_

---

_Comment by @zanieb on 2024-05-18 17:00_

Makes sense to me, but deferring to Andrew.

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-05-18 17:19_

---

_@BurntSushi approved on 2024-05-20 12:24_

Makes sense to me too.

---

_Merged by @charliermarsh on 2024-05-20 13:25_

---

_Closed by @charliermarsh on 2024-05-20 13:25_

---

_Branch deleted on 2024-05-20 13:25_

---
