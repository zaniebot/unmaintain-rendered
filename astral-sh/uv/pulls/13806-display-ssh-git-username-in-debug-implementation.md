```yaml
number: 13806
title: "Display ssh git username in `Debug` implementation"
type: pull_request
state: merged
author: jtfmumm
labels:
  - bug
assignees: []
merged: true
base: main
head: jtfm/ssh-git-username
created_at: 2025-06-03T13:19:12Z
updated_at: 2025-06-03T14:52:52Z
url: https://github.com/astral-sh/uv/pull/13806
synced_at: 2026-01-10T11:10:42Z
```

# Display ssh git username in `Debug` implementation

---

_Pull request opened by @jtfmumm on 2025-06-03 13:19_

This updates the `Debug` implementation of `DisplaySafeUrl` to display the git username in a case like `git+ssh://git@github.com/org/repo`. It also factors out the git username check into a shared function and adds related unit tests to `DisplaySafeUrl`.


---

_Marked ready for review by @jtfmumm on 2025-06-03 13:27_

---

_Review requested from @konstin by @jtfmumm on 2025-06-03 13:27_

---

_Review comment by @konstin on `crates/uv-redacted/src/lib.rs`:284 on 2025-06-03 14:00_

nit: `.to_string()`

---

_@konstin reviewed on 2025-06-03 14:00_

---

_Label `bug` added by @jtfmumm on 2025-06-03 14:01_

---

_@zanieb approved on 2025-06-03 14:30_

---

_@konstin approved on 2025-06-03 14:31_

---

_Merged by @zanieb on 2025-06-03 14:52_

---

_Closed by @zanieb on 2025-06-03 14:52_

---

_Branch deleted on 2025-06-03 14:52_

---
