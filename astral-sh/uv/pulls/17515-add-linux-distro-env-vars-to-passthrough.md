```yaml
number: 17515
title: Add linux distro env vars to passthrough
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/gentoo-passthrough
created_at: 2026-01-16T12:43:23Z
updated_at: 2026-01-20T09:54:12Z
url: https://github.com/astral-sh/uv/pull/17515
synced_at: 2026-01-20T10:43:38Z
```

# Add linux distro env vars to passthrough

---

_@konstin_

See #17509



---

_Label `internal` added by @konstin on 2026-01-16 12:43_

---

_@zanieb approved on 2026-01-16 13:11_

---

_Comment by @konstin on 2026-01-16 16:55_

From looking at the XDG vars, `XDG_CONFIG_DIRS` seems to be the only one that we use and that doesn't default to `HOME`.

`PATH` abnd `XDG_CONFIG_DIRS` seem to be the two main problems for linux distros, we can start with those and then iterate.

---

_@zanieb reviewed on 2026-01-16 17:15_

---

_Review comment by @zanieb on `crates/uv/tests/it/common/mod.rs`:1020 on 2026-01-16 17:15_

Can we join a child directory as we do below?

---

_@mgorny approved on 2026-01-16 17:59_

LGTM, thanks! It fixes almost all my issues.

---

_Merged by @konstin on 2026-01-20 09:54_

---

_Closed by @konstin on 2026-01-20 09:54_

---

_Branch deleted on 2026-01-20 09:54_

---
