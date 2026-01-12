```yaml
number: 2312
title: Move system install tests into normal CI
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/system-install-combine
created_at: 2024-03-09T00:39:46Z
updated_at: 2024-03-12T05:30:43Z
url: https://github.com/astral-sh/uv/pull/2312
synced_at: 2026-01-12T16:04:58Z
```

# Move system install tests into normal CI

---

_@zanieb_

Giving this a try... just making all of these a normal part of CI.

This is probably slightly slower than our normal CI, but not by much (it depends how bad of a roll we get on the Windows network performance). Includes #2309 to reduce the overhead of adding more platforms.

Alternatively, we could gate these with a label and just run on main by default (i.e. #2308)



---

_Label `testing` added by @zanieb on 2024-03-09 00:40_

---

_Marked ready for review by @zanieb on 2024-03-11 23:28_

---

_Review requested from @charliermarsh by @zanieb on 2024-03-11 23:29_

---

_Comment by @zanieb on 2024-03-11 23:30_

#2370 removes the extra `debian` build step

---

_Merged by @zanieb on 2024-03-12 05:30_

---

_Closed by @zanieb on 2024-03-12 05:30_

---

_Branch deleted on 2024-03-12 05:30_

---
