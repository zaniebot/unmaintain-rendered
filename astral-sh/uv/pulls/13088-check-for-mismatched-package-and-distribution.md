```yaml
number: 13088
title: Check for mismatched package and distribution names on resolver thread
type: pull_request
state: merged
author: jtfmumm
labels:
  - bug
assignees: []
merged: true
base: main
head: jtfm/bogus-redirect-hang
created_at: 2025-04-24T12:59:26Z
updated_at: 2025-04-24T13:18:03Z
url: https://github.com/astral-sh/uv/pull/13088
synced_at: 2026-01-10T11:10:40Z
```

# Check for mismatched package and distribution names on resolver thread

---

_Pull request opened by @jtfmumm on 2025-04-24 12:59_

This PR restores the `bogus_redirect` test that was non-deterministically hanging (reverting #13076). 

Mismatched package and distribution names were causing uv to hang prior to #12917 (which added the `bogus_redirect` test). But with that fix, uv was only checking for mismatched package names on the main thread (and not the resolver thread). This allowed for a race condition which would prevent uv from ever doing the check, triggering the original hang condition. This PR adds the check to the resolver thread to prevent this race condition.


---

_Label `bug` added by @jtfmumm on 2025-04-24 12:59_

---

_@konstin approved on 2025-04-24 13:03_

---

_Merged by @jtfmumm on 2025-04-24 13:18_

---

_Closed by @jtfmumm on 2025-04-24 13:18_

---

_Branch deleted on 2025-04-24 13:18_

---
