```yaml
number: 559
title: Remove existing built wheels when building source distributions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/rm
created_at: 2023-12-05T01:47:43Z
updated_at: 2023-12-05T17:45:26Z
url: https://github.com/astral-sh/uv/pull/559
synced_at: 2026-01-12T16:04:02Z
```

# Remove existing built wheels when building source distributions

---

_@charliermarsh_

This PR modifies the source distribution building to replace any existing targets after building the new wheel. In some cases, the existence of an existing target may be indicative of a bug, so we warn. It's partially a workaround for some (but not all) of the errors in https://github.com/astral-sh/puffin/issues/554.


---

_Review requested from @konstin by @charliermarsh on 2023-12-05 01:47_

---

_Label `bug` added by @charliermarsh on 2023-12-05 01:47_

---

_Comment by @charliermarsh on 2023-12-05 01:58_

When we repeat these builds, it's likely a caching bug. But I'd rather clear the cache than fail entirely.

---

_Comment by @charliermarsh on 2023-12-05 17:45_

We agreed synchronously to merge this for now, though we're going to revisit this decision when we start separating zipped and unzipped wheels.

---

_Merged by @charliermarsh on 2023-12-05 17:45_

---

_Closed by @charliermarsh on 2023-12-05 17:45_

---

_Branch deleted on 2023-12-05 17:45_

---
