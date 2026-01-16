```yaml
number: 22477
title: Use explicit manylinux/musllinux targets and better pre-upload checks
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/better-manylinux-maturin-handling
created_at: 2026-01-09T12:36:39Z
updated_at: 2026-01-16T14:26:54Z
url: https://github.com/astral-sh/ruff/pull/22477
synced_at: 2026-01-16T14:57:55Z
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

_@ntBre approved on 2026-01-16 14:21_

Thank you!

---

_Closed by @ntBre on 2026-01-16 14:22_

---

_Reopened by @ntBre on 2026-01-16 14:22_

---

_Comment by @ntBre on 2026-01-16 14:23_

I guess I'll take the blame on this one :laughing: (https://github.com/astral-sh/ty/pull/2393#issuecomment-3759138998).

---

_Merged by @ntBre on 2026-01-16 14:26_

---

_Closed by @ntBre on 2026-01-16 14:26_

---

_Branch deleted on 2026-01-16 14:26_

---
