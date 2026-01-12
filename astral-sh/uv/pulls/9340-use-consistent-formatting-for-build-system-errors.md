```yaml
number: 9340
title: Use consistent formatting for build system errors
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/format
created_at: 2024-11-21T23:05:42Z
updated_at: 2024-11-27T14:22:55Z
url: https://github.com/astral-sh/uv/pull/9340
synced_at: 2026-01-12T16:08:45Z
```

# Use consistent formatting for build system errors

---

_@charliermarsh_

## Summary

These look pretty different from the help / hint messages we show on resolver failure. I've added color, backticks, and a "hint:" prefix.

Before:

![Screenshot 2024-11-21 at 5 45 40 PM](https://github.com/user-attachments/assets/71ee2551-8fc6-4715-a9d7-68055cd34547)

After:

![Screenshot 2024-11-21 at 6 05 31 PM](https://github.com/user-attachments/assets/24e7fb46-49aa-4b6f-a442-115a71a3414b)


---

_@charliermarsh reviewed on 2024-11-21 23:09_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/report.rs`:1111 on 2024-11-21 23:09_

Should the package names be cyan? In the build failures, they're now cyan (but not bold)... But here, they're just bold.

---

_@charliermarsh reviewed on 2024-11-22 03:02_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/report.rs`:1071 on 2024-11-22 03:02_

Maybe this is bad? We don't use backticks around the package name in the PubGrub error message (just this hint, now).

---

_Review requested from @zanieb by @charliermarsh on 2024-11-26 21:25_

---

_Label `error messages` added by @charliermarsh on 2024-11-26 21:25_

---

_@zanieb reviewed on 2024-11-26 22:34_

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/report.rs`:1071 on 2024-11-26 22:34_

I think that's consistent with everything else. The PubGrub message is the only place we omit the backtricks, I'm not sure what to do about that in the long-term.

---

_@zanieb reviewed on 2024-11-26 22:34_

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/report.rs`:1111 on 2024-11-26 22:34_

Consistency seems nice. I don't have strong opinions on coloring.

---

_@zanieb approved on 2024-11-26 22:35_

---

_Merged by @charliermarsh on 2024-11-27 14:22_

---

_Closed by @charliermarsh on 2024-11-27 14:22_

---

_Branch deleted on 2024-11-27 14:22_

---
