```yaml
number: 11166
title: "uv-install-wheel: Split installation logic and link logic"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/split-wheel-install-into-installer-and-linker
created_at: 2025-02-02T14:54:12Z
updated_at: 2025-02-02T15:02:14Z
url: https://github.com/astral-sh/uv/pull/11166
synced_at: 2026-01-10T11:10:34Z
```

# uv-install-wheel: Split installation logic and link logic

---

_Pull request opened by @konstin on 2025-02-02 14:54_

uv-install-wheel had the logic for laying out the installation and for linking a directory in the same module. We split them up to isolate each module's logic and tighten the crate's interface to only expose top level members.

No logic changes, only moving code around.

---

_Label `internal` added by @konstin on 2025-02-02 14:54_

---

_Merged by @konstin on 2025-02-02 15:02_

---

_Closed by @konstin on 2025-02-02 15:02_

---

_Branch deleted on 2025-02-02 15:02_

---
