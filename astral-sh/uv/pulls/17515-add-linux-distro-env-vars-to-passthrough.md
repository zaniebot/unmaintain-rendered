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
updated_at: 2026-01-16T16:55:55Z
url: https://github.com/astral-sh/uv/pull/17515
synced_at: 2026-01-16T16:59:58Z
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
