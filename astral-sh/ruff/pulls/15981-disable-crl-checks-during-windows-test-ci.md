```yaml
number: 15981
title: Disable CRL checks during Windows test CI
type: pull_request
state: closed
author: zanieb
labels:
  - ci
assignees: []
draft: true
base: main
head: zb/fix-windows
created_at: 2025-02-05T20:58:14Z
updated_at: 2025-02-10T23:49:34Z
url: https://github.com/astral-sh/ruff/pull/15981
synced_at: 2026-01-10T19:57:22Z
```

# Disable CRL checks during Windows test CI

---

_Pull request opened by @zanieb on 2025-02-05 20:58_

Corresponding to https://github.com/astral-sh/uv/pull/11262

---

_Label `ci` added by @zanieb on 2025-02-05 20:58_

---

_Comment by @zanieb on 2025-02-05 21:03_

per https://github.com/astral-sh/uv/pull/11262#issuecomment-2638017165 unclear if this is actually working

---

_Comment by @github-actions[bot] on 2025-02-05 21:07_

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

_Comment by @zanieb on 2025-02-05 22:19_

From what I can tell, I was defeated and only the fact that the failure is spurious made this look effective.

---

_Closed by @zanieb on 2025-02-10 23:49_

---
