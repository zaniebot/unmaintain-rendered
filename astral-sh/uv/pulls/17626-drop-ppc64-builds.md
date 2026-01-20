```yaml
number: 17626
title: Drop PPC64 builds
type: pull_request
state: open
author: konstin
labels:
  - breaking
  - "build:skip-release"
assignees: []
base: main
head: konsti/drop-ppc64
created_at: 2026-01-20T15:08:46Z
updated_at: 2026-01-20T15:15:43Z
url: https://github.com/astral-sh/uv/pull/17626
synced_at: 2026-01-20T15:44:17Z
```

# Drop PPC64 builds

---

_@konstin_

PPC64 seems dead, and it's only supported on one exact manylinux version (https://github.com/pypa/auditwheel/issues/669), so we should drop it.

This is a low risk change, but we can wait to 0.10 also.


---

_Added to milestone `v0.10.0` by @konstin on 2026-01-20 15:08_

---

_Label `breaking` added by @konstin on 2026-01-20 15:08_

---

_Comment by @zanieb on 2026-01-20 15:11_

Do we need to update https://docs.astral.sh/uv/reference/policies/platforms/

---

_Label `build:skip-release` added by @zanieb on 2026-01-20 15:15_

---
