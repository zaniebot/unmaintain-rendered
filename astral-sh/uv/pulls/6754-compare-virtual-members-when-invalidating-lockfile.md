```yaml
number: 6754
title: Compare virtual members when invalidating lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/conv
created_at: 2024-08-28T14:57:28Z
updated_at: 2024-08-28T15:11:18Z
url: https://github.com/astral-sh/uv/pull/6754
synced_at: 2026-01-12T16:07:31Z
```

# Compare virtual members when invalidating lockfile

---

_@charliermarsh_

## Summary

Whether a package is itself virtual isn't captured in the package metadata, so we have to compare the sources.

Closes https://github.com/astral-sh/uv/issues/6749.


---

_Review requested from @zanieb by @charliermarsh on 2024-08-28 14:57_

---

_Label `bug` added by @charliermarsh on 2024-08-28 14:57_

---

_@zanieb approved on 2024-08-28 15:00_

Oo I was right.

---

_Merged by @charliermarsh on 2024-08-28 15:11_

---

_Closed by @charliermarsh on 2024-08-28 15:11_

---

_Branch deleted on 2024-08-28 15:11_

---
