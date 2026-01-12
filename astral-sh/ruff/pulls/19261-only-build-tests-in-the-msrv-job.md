```yaml
number: 19261
title: Only build tests in the msrv job
type: pull_request
state: merged
author: zanieb
labels:
  - ci
assignees: []
merged: true
base: main
head: zb/msrv-build
created_at: 2025-07-10T13:43:33Z
updated_at: 2025-07-11T14:16:15Z
url: https://github.com/astral-sh/ruff/pull/19261
synced_at: 2026-01-12T15:56:35Z
```

# Only build tests in the msrv job

---

_@zanieb_

Alternative to https://github.com/astral-sh/ruff/pull/19260

---

_Label `ci` added by @zanieb on 2025-07-10 13:43_

---

_Comment by @github-actions[bot] on 2025-07-10 13:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @zanieb on 2025-07-10 15:54_

---

_Review requested from @MichaReiser by @zanieb on 2025-07-10 18:25_

---

_@MichaReiser reviewed on 2025-07-11 07:34_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:414 on 2025-07-11 07:34_

I think the `--all-features` is still important to ensure we build all code.

---

_@zanieb reviewed on 2025-07-11 12:49_

---

_Review comment by @zanieb on `.github/workflows/ci.yaml`:414 on 2025-07-11 12:49_

Indeed, sorry I thought about retaining it but missed it still!

---

_@MichaReiser approved on 2025-07-11 13:29_

Thank you

---

_Merged by @zanieb on 2025-07-11 14:16_

---

_Closed by @zanieb on 2025-07-11 14:16_

---

_Branch deleted on 2025-07-11 14:16_

---
