```yaml
number: 15564
title: Ensure we get the last error from Windows on the same thread
type: pull_request
state: merged
author: zanieb
labels:
  - bug
  - internal
  - windows
assignees: []
merged: true
base: main
head: zb/auth-win-safety
created_at: 2025-08-27T21:18:17Z
updated_at: 2025-08-27T21:42:38Z
url: https://github.com/astral-sh/uv/pull/15564
synced_at: 2026-01-12T16:11:49Z
```

# Ensure we get the last error from Windows on the same thread

---

_@zanieb_

Reverts #15552
Closes https://github.com/astral-sh/uv/pull/15562
Closes https://github.com/astral-sh/uv/issues/15558

The `GetLastError` calls must be on the same thread, or we can pull the wrong last error!

---

_Label `bug` added by @zanieb on 2025-08-27 21:18_

---

_Review comment by @zanieb on `crates/uv-keyring/src/windows.rs`:171 on 2025-08-27 21:21_

Notice here we called `decode_error` which invokes `GetLastError` _outside_ of the credential delete thread

---

_@zanieb reviewed on 2025-08-27 21:21_

---

_Label `windows` added by @zanieb on 2025-08-27 21:21_

---

_Label `preview` added by @zanieb on 2025-08-27 21:21_

---

_Label `preview` removed by @zanieb on 2025-08-27 21:21_

---

_Label `internal` added by @zanieb on 2025-08-27 21:21_

---

_Marked ready for review by @zanieb on 2025-08-27 21:21_

---

_@zanieb reviewed on 2025-08-27 21:33_

---

_Review comment by @zanieb on `crates/uv-keyring/src/windows.rs`:171 on 2025-08-27 21:33_

cc @oconnor663 just so you're aware of the gotcha in the future :)

---

_Merged by @zanieb on 2025-08-27 21:42_

---

_Closed by @zanieb on 2025-08-27 21:42_

---

_Branch deleted on 2025-08-27 21:42_

---
