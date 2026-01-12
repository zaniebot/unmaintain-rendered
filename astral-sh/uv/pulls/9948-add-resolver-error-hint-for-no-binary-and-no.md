```yaml
number: 9948
title: Add resolver error hint for no-binary and no-build failures
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/no-build-bin
created_at: 2024-12-16T22:56:22Z
updated_at: 2024-12-17T00:47:42Z
url: https://github.com/astral-sh/uv/pull/9948
synced_at: 2026-01-12T16:09:03Z
```

# Add resolver error hint for no-binary and no-build failures

---

_@zanieb_

Moves some of the context out of the error chain to improve readability.

---

_Label `error messages` added by @zanieb on 2024-12-16 22:56_

---

_Marked ready for review by @zanieb on 2024-12-16 23:57_

---

_@charliermarsh reviewed on 2024-12-17 00:20_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_install.rs`:2195 on 2024-12-17 00:20_

I think "because building from source is disabled" would be more natural

---

_@charliermarsh approved on 2024-12-17 00:20_

---

_Merged by @zanieb on 2024-12-17 00:47_

---

_Closed by @zanieb on 2024-12-17 00:47_

---

_Branch deleted on 2024-12-17 00:47_

---
