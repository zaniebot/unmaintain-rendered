```yaml
number: 17515
title: Add linux distro env vars to passthrough
type: pull_request
state: open
author: konstin
labels:
  - internal
assignees: []
base: main
head: konsti/gentoo-passthrough
created_at: 2026-01-16T12:43:23Z
updated_at: 2026-01-16T17:15:30Z
url: https://github.com/astral-sh/uv/pull/17515
synced_at: 2026-01-16T18:01:14Z
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
