```yaml
number: 13814
title: "Test case: Don't redact username `git` for SSH"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/test-dont-redact-ssh-git-username
created_at: 2025-06-03T14:08:25Z
updated_at: 2025-06-04T14:54:01Z
url: https://github.com/astral-sh/uv/pull/13814
synced_at: 2026-01-10T11:10:42Z
```

# Test case: Don't redact username `git` for SSH

---

_Pull request opened by @konstin on 2025-06-03 14:08_

_No description provided._

---

_Label `internal` added by @konstin on 2025-06-03 14:08_

---

_Merged by @konstin on 2025-06-04 07:57_

---

_Closed by @konstin on 2025-06-04 07:57_

---

_Branch deleted on 2025-06-04 07:57_

---

_Comment by @zanieb on 2025-06-04 14:32_

This is kind of a scary pull request to merge without any review!

---

_@zanieb reviewed on 2025-06-04 14:33_

---

_Review comment by @zanieb on `crates/uv/tests/it/common/mod.rs`:1629 on 2025-06-04 14:33_

Can you name this with the scope? i.e., `READ_ONLY_GITHUB_SSH_DEPLOY_KEY` or `GITHUB_SSH_DEPLOY_KEY`?

---

_@zanieb reviewed on 2025-06-04 14:34_

---

_Review comment by @zanieb on `crates/uv/tests/it/export.rs`:1180 on 2025-06-04 14:34_

Maybe `reduce_ssh_key_file_permissions`? It's pretty vague as written.

---

_Review comment by @zanieb on `crates/uv/tests/it/export.rs`:1179 on 2025-06-04 14:34_

It'd be nice to at least briefly say what we're going to do to solve the problem here.

---

_@zanieb reviewed on 2025-06-04 14:34_

---

_@zanieb reviewed on 2025-06-04 14:35_

---

_Review comment by @zanieb on `crates/uv/tests/it/export.rs`:1192 on 2025-06-04 14:35_

Minor note this isn't tested in CI because we skip the `git` feature 

---

_@zanieb reviewed on 2025-06-04 14:36_

---

_Review comment by @zanieb on `crates/uv/tests/it/export.rs`:1228 on 2025-06-04 14:36_

It's present, just invalid, right?

---

_@zanieb reviewed on 2025-06-04 14:36_

---

_Review comment by @zanieb on `crates/uv/tests/it/export.rs`:1236 on 2025-06-04 14:36_

Thank you for using the `[<CONTENT>]` format, it's nice in the snapshot :)

---

_Review comment by @zanieb on `crates/uv/tests/it/common/mod.rs`:1629 on 2025-06-04 14:37_

Is the key read only? 

---

_@zanieb reviewed on 2025-06-04 14:37_

---

_@konstin reviewed on 2025-06-04 14:48_

---

_Review comment by @konstin on `crates/uv/tests/it/export.rs`:1192 on 2025-06-04 14:48_

Yeah, this is only for local Windows development. We can iterate over the command if it fails for specific Windows setups.

---

_Review comment by @konstin on `crates/uv/tests/it/common/mod.rs`:1629 on 2025-06-04 14:54_

yes

---

_@konstin reviewed on 2025-06-04 14:54_

---
