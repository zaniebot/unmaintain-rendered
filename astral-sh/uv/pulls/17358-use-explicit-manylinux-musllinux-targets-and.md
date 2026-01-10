```yaml
number: 17358
title: Use explicit manylinux/musllinux targets and better pre-upload checks
type: pull_request
state: open
author: konstin
labels:
  - internal
assignees: []
base: main
head: konsti/explicit-manylinux
created_at: 2026-01-08T10:02:14Z
updated_at: 2026-01-09T13:51:23Z
url: https://github.com/astral-sh/uv/pull/17358
synced_at: 2026-01-10T05:49:14Z
```

# Use explicit manylinux/musllinux targets and better pre-upload checks

---

_Pull request opened by @konstin on 2026-01-08 10:02_

This ensures that changes to the targets are intentional and explicit.

The target versions are from https://pypi.org/project/uv/0.9.22/#files

See also https://github.com/astral-sh/ty/pull/2393

---

_Label `internal` added by @konstin on 2026-01-08 10:02_

---

_Renamed from "Use explicit manylinux/musllinux target" to "Use explicit manylinux/musllinux targets" by @konstin on 2026-01-08 10:02_

---

_Comment by @konstin on 2026-01-08 10:02_

Test run: https://github.com/astral-sh/uv/actions/runs/20812975443

---

_Converted to draft by @konstin on 2026-01-08 10:59_

---

_Renamed from "Use explicit manylinux/musllinux targets" to "Use explicit manylinux/musllinux targets and better pre-upload checks" by @konstin on 2026-01-09 12:05_

---

_Marked ready for review by @konstin on 2026-01-09 12:35_

---

_Review requested from @zanieb by @konstin on 2026-01-09 12:49_

---

_Comment by @zanieb on 2026-01-09 12:59_

Is it intentional to also upgrade maturin here?

---

_Comment by @konstin on 2026-01-09 13:26_

Yes, we need the new version for the better checks (`--compatibility pypi` with `manylinux: `)

---

_@zanieb approved on 2026-01-09 13:51_

---
