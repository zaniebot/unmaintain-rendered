```yaml
number: 14465
title: Specify the wasm-pack version explicitly
type: pull_request
state: merged
author: zanieb
labels:
  - ci
assignees: []
merged: true
base: main
head: zb/wasm-version
created_at: 2024-11-19T17:50:16Z
updated_at: 2024-11-20T00:00:29Z
url: https://github.com/astral-sh/ruff/pull/14465
synced_at: 2026-01-12T15:55:47Z
```

# Specify the wasm-pack version explicitly

---

_@zanieb_

There is an upstream bug with latest version detection https://github.com/jetli/wasm-pack-action/issues/23

This causes random flakes of the wasm build job.

---

_Label `ci` added by @zanieb on 2024-11-19 17:50_

---

_Comment by @github-actions[bot] on 2024-11-19 18:09_

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

_@AlexWaygood approved on 2024-11-19 22:48_

Well tracked down, thanks!

---

_Merged by @zanieb on 2024-11-20 00:00_

---

_Closed by @zanieb on 2024-11-20 00:00_

---

_Branch deleted on 2024-11-20 00:00_

---
