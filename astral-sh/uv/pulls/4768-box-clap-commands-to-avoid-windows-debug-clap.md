```yaml
number: 4768
title: Box clap commands to avoid windows debug clap stack overflow
type: pull_request
state: merged
author: konstin
labels:
  - internal
  - windows
assignees: []
merged: true
base: main
head: konsti/box-clap-arguments
created_at: 2024-07-03T12:58:04Z
updated_at: 2024-07-03T17:06:17Z
url: https://github.com/astral-sh/uv/pull/4768
synced_at: 2026-01-12T16:06:26Z
```

# Box clap commands to avoid windows debug clap stack overflow

---

_@konstin_

The changes in https://github.com/astral-sh/uv/pull/4708 caused an overflow in debug mode only of the 1MB default stack size in windows during clap. This means that even trivial wrong argument tests would fail without increasing the stack size. As remedy, we box the clap types.

---

_Label `internal` added by @konstin on 2024-07-03 12:58_

---

_Label `windows` added by @konstin on 2024-07-03 12:58_

---

_Comment by @zanieb on 2024-07-03 12:59_

I feel like Ibraheem said the size was still not that big though? Where is the rest of the stack being used?

---

_Comment by @konstin on 2024-07-03 13:04_

The stack size was not the problem, but the tests that didn't set the stack size because they were only testing clap behavior, as you can see in https://github.com/astral-sh/uv/actions/runs/9777625178/job/26992714658.

---

_Comment by @konstin on 2024-07-03 13:33_

I've confirmed in https://github.com/astral-sh/uv/actions/runs/9778590193/job/26995771136?pr=4765 that this unblocks #4708 and we can return to the 8MB stack size.

---

_@zanieb approved on 2024-07-03 13:58_

---

_@ibraheemdev approved on 2024-07-03 16:56_

---

_Merged by @konstin on 2024-07-03 17:06_

---

_Closed by @konstin on 2024-07-03 17:06_

---

_Branch deleted on 2024-07-03 17:06_

---
