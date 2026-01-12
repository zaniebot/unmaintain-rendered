```yaml
number: 15472
title: "Stop setting `CLICOLOR_FORCE=1` when calling build backends"
type: pull_request
state: merged
author: zsol
labels:
  - bug
assignees: []
merged: true
base: main
head: zsol/jj-kwnswvnoxxpw
created_at: 2025-08-23T16:08:47Z
updated_at: 2025-08-27T17:32:40Z
url: https://github.com/astral-sh/uv/pull/15472
synced_at: 2026-01-12T16:11:45Z
```

# Stop setting `CLICOLOR_FORCE=1` when calling build backends

---

_@zsol_

## Summary

`CLICOLOR_FORCE` changes the output of underlying build commands, which messes with wrapper tools trying to parse their output.

Closes #12564, closes #15415. 

---

_Review requested from @konstin by @zanieb on 2025-08-23 18:53_

---

_Assigned to @konstin by @zanieb on 2025-08-23 18:53_

---

_Marked ready for review by @zsol on 2025-08-24 10:42_

---

_Comment by @charliermarsh on 2025-08-24 12:16_

Looks like this was added in #3032.

---

_Comment by @charliermarsh on 2025-08-24 12:17_

It's kind of a bummer to lose this, since it seems like a bug in the respect backends, but I agree that failing is worse.

---

_Comment by @konstin on 2025-08-25 10:30_

Do we have upstream reports in the affected build backends? It would be nice to move towards having color again.

---

_@konstin approved on 2025-08-25 10:30_

---

_Label `bug` added by @konstin on 2025-08-25 10:30_

---

_Renamed from "build-frontend: stop setting CLICOLOR_FORCE=1 while building" to "Stop setting `CLICOLOR_FORCE=1` when calling build backends" by @konstin on 2025-08-25 10:31_

---

_Comment by @zsol on 2025-08-27 15:27_

I've raised this in the cmake repo at https://gitlab.kitware.com/cmake/cmake/-/issues/27173 but I highly suspect this is going to be an endless game of whack-a-mole. CMake's CUDA plugin also suffers from a similar issue: https://gitlab.kitware.com/cmake/cmake/-/issues/27137

---

_Merged by @zsol on 2025-08-27 15:28_

---

_Closed by @zsol on 2025-08-27 15:28_

---

_Branch deleted on 2025-08-27 15:28_

---

_Comment by @thomasbbrunner on 2025-08-27 17:32_

Just noting that apparently this has already been fixed in cmake: https://gitlab.kitware.com/cmake/cmake/-/merge_requests/11080

See comment in https://gitlab.kitware.com/cmake/cmake/-/issues/27173

---
