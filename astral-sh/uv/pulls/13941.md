```yaml
number: 13941
title: Try more random PowerShell things to catch Windows CI crashes
type: pull_request
state: closed
author: konstin
labels:
  - ci-flake
assignees: []
base: main
head: konsti/much-more-powershell
created_at: 2025-06-10T10:24:16Z
updated_at: 2025-07-24T10:00:16Z
url: https://github.com/astral-sh/uv/pull/13941
synced_at: 2026-01-10T06:53:01Z
```

# Try more random PowerShell things to catch Windows CI crashes

---

_Pull request opened by @konstin on 2025-06-10 10:24_

As I'm out of ideas otherwise, I've generated even more extensive powershell code for the unexplainable Windows CI crashes in https://github.com/astral-sh/uv/issues/13812. This code should be deleted once we figured out the crash.

---

_Label `ci-flake` added by @konstin on 2025-06-10 10:24_

---

_Renamed from "Try more random powershell things to catch Windows CI crashes" to "Try more random PowerShell things to catch Windows CI crashes" by @konstin on 2025-06-10 11:10_

---

_Comment by @zanieb on 2025-06-10 13:09_

Is there a way we can try to reproduce this here? This is kind of frightening to merge

---

_Comment by @konstin on 2025-06-10 13:16_

This code exists because we cannot reproduce the failure in https://github.com/astral-sh/uv/issues/13812, I've never seen this status code outside of our CI, so I'm not sure what the alternative to trying this on CI is.

---

_Comment by @zanieb on 2025-06-10 13:19_

I'm suggesting running the tests on a loop here or something, instead of merging to `main`.

---

_Comment by @konstin on 2025-06-12 07:55_

I ran the script in a loop two times for 6h to no effect.

---

_Comment by @konstin on 2025-07-24 10:00_

We don't need that anymore :tada:

---

_Closed by @konstin on 2025-07-24 10:00_

---
