```yaml
number: 13799
title: "Don't redact username `git` for SSH"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/dont-redact-ssh-git-username
created_at: 2025-06-03T09:32:24Z
updated_at: 2025-06-03T14:18:21Z
url: https://github.com/astral-sh/uv/pull/13799
synced_at: 2026-01-10T11:10:42Z
```

# Don't redact username `git` for SSH

---

_Pull request opened by @konstin on 2025-06-03 09:32_

Fixes #13795 specifically by not redacting the username convention `git` for SSH URLs, though we should never write stars in `uv export` generally (#13791).

---

_Review requested from @jtfmumm by @konstin on 2025-06-03 09:32_

---

_Label `bug` added by @konstin on 2025-06-03 09:32_

---

_@jtfmumm approved on 2025-06-03 10:55_

---

_Review comment by @zanieb on `crates/uv/tests/it/export.rs`:4107 on 2025-06-03 12:46_

To match the other tests in the file 
```suggestion
fn requirements_txt_ssh_git_username() -> Result<()> {
```

---

_@zanieb reviewed on 2025-06-03 12:46_

---

_@zanieb reviewed on 2025-06-03 12:47_

---

_Review comment by @zanieb on `crates/uv/tests/it/export.rs`:4107 on 2025-06-03 12:47_

Can you move it next to 
https://github.com/astral-sh/uv/pull/13792

---

_@zanieb approved on 2025-06-03 14:07_

---

_Merged by @zanieb on 2025-06-03 14:18_

---

_Closed by @zanieb on 2025-06-03 14:18_

---

_Branch deleted on 2025-06-03 14:18_

---
