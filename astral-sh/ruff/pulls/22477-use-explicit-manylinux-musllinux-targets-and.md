```yaml
number: 22477
title: Use explicit manylinux/musllinux targets and better pre-upload checks
type: pull_request
state: open
author: konstin
labels:
  - internal
assignees: []
base: main
head: konsti/better-manylinux-maturin-handling
created_at: 2026-01-09T12:36:39Z
updated_at: 2026-01-16T09:54:35Z
url: https://github.com/astral-sh/ruff/pull/22477
synced_at: 2026-01-16T09:55:10Z
```

# Use explicit manylinux/musllinux targets and better pre-upload checks

---

_@konstin_

This ensures that changes to the targets are intentional and explicit.

See also https://github.com/astral-sh/ty/pull/2393 and https://github.com/astral-sh/uv/pull/17358

---

_Comment by @konstin on 2026-01-16 09:02_

I've verified these changes with testpypi: https://test.pypi.org/project/ruff-konsti-test/#files

I'll leave merging to the person who makes the release.

---

_Marked ready for review by @konstin on 2026-01-16 09:02_

---

_Label `internal` added by @konstin on 2026-01-16 09:02_

---

_Review requested from @ntBre by @MichaReiser on 2026-01-16 09:54_

---

_@MichaReiser approved on 2026-01-16 09:54_

I don't understand much of this but overall this makes sense to me.

Thank you

---
