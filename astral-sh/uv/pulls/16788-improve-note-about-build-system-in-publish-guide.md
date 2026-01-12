```yaml
number: 16788
title: Improve note about build system in publish guide
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/build-system
created_at: 2025-11-20T14:54:35Z
updated_at: 2025-11-20T20:00:01Z
url: https://github.com/astral-sh/uv/pull/16788
synced_at: 2026-01-12T16:12:26Z
```

# Improve note about build system in publish guide

---

_@zanieb_

Addresses https://github.com/astral-sh/uv/issues/5605#issuecomment-3549958048

---

_Label `documentation` added by @zanieb on 2025-11-20 14:54_

---

_Review requested from @konstin by @zanieb on 2025-11-20 17:28_

---

_Review comment by @konstin on `docs/guides/package.md`:17 on 2025-11-20 19:55_

The user that said that we have the fallback behavior is technically correct, and we do build if there's a workspace member (or any other package) without build system with another workspace member depending on it, but the real answer is that you really want to configure a build system, so I'm not worried about this being technically wrong.

---

_@konstin approved on 2025-11-20 19:55_

---

_@zanieb reviewed on 2025-11-20 19:59_

---

_Review comment by @zanieb on `docs/guides/package.md`:17 on 2025-11-20 19:59_

Fair, thanks for the clarification

---

_Merged by @zanieb on 2025-11-20 19:59_

---

_Closed by @zanieb on 2025-11-20 19:59_

---

_Branch deleted on 2025-11-20 20:00_

---
