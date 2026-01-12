```yaml
number: 5890
title: Use a larger runner for Windows test jobs
type: pull_request
state: closed
author: zanieb
labels:
  - testing
assignees: []
base: main
head: zb/ci-windows-xxl
created_at: 2024-08-07T20:11:00Z
updated_at: 2024-08-08T14:11:15Z
url: https://github.com/astral-sh/uv/pull/5890
synced_at: 2026-01-12T16:07:05Z
```

# Use a larger runner for Windows test jobs

---

_@zanieb_

From 16 to 32 cores.

Following https://github.com/astral-sh/uv/pull/5873
Part of #5713


---

_Label `testing` added by @zanieb on 2024-08-07 20:11_

---

_Comment by @zanieb on 2024-08-07 20:20_

From 6m 50s to 6m 19s. Hm. 5m 2s on a re-run.

---

_Comment by @zanieb on 2024-08-07 20:42_

And 4m 33s on another run. Hm.

---

_Comment by @zanieb on 2024-08-07 21:33_

Happy to hear thoughts. I can project the expect increase in monthly costs, but it's a bit tedious. This is another 2x more expensive than the current runner.

---

_Marked ready for review by @zanieb on 2024-08-07 21:34_

---

_Comment by @zanieb on 2024-08-08 14:10_

After rebasing, 4m 26s

---

_Comment by @zanieb on 2024-08-08 14:11_

Not actually faster than `main` afaict. Maybe more reliable? Can revisit.

---

_Closed by @zanieb on 2024-08-08 14:11_

---
