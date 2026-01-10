```yaml
number: 15827
title: "Allow cached environment reuse with `@latest`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/re
created_at: 2025-09-14T01:03:45Z
updated_at: 2025-09-14T13:10:48Z
url: https://github.com/astral-sh/uv/pull/15827
synced_at: 2026-01-10T06:36:15Z
```

# Allow cached environment reuse with `@latest`

---

_Pull request opened by @charliermarsh on 2025-09-14 01:03_

## Summary

I think this is leftover from a prior refactor whereby we used to avoid reusing the cached environment if `--reinstall` was passed; but then we stopped allowing `--reinstall` in `uv tool run` anyway, and this got changed to `--refresh`. It seems wrong to skip cache reuse with `--refresh`, though.

Closes https://github.com/astral-sh/uv/issues/15824.


---

_Review requested from @zanieb by @charliermarsh on 2025-09-14 01:04_

---

_Review requested from @konstin by @charliermarsh on 2025-09-14 01:04_

---

_Label `bug` added by @charliermarsh on 2025-09-14 01:04_

---

_Marked ready for review by @charliermarsh on 2025-09-14 01:04_

---

_@domdomegg approved on 2025-09-14 01:10_

this looks correct, although I'm not super familiar with the codebase

---

_@konstin approved on 2025-09-14 11:23_

---

_Merged by @charliermarsh on 2025-09-14 13:10_

---

_Closed by @charliermarsh on 2025-09-14 13:10_

---

_Branch deleted on 2025-09-14 13:10_

---
