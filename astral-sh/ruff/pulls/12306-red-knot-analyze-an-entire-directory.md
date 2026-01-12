```yaml
number: 12306
title: "[red-knot] Analyze an entire directory"
type: pull_request
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
draft: true
base: system-walk-directories
head: red-knot-directories
created_at: 2024-07-12T18:14:44Z
updated_at: 2024-08-12T07:52:23Z
url: https://github.com/astral-sh/ruff/pull/12306
synced_at: 2026-01-12T15:55:40Z
```

# [red-knot] Analyze an entire directory

---

_@MichaReiser_

_No description provided._

---

_Label `red-knot` added by @MichaReiser on 2024-07-12 18:15_

---

_@MichaReiser reviewed on 2024-07-12 18:17_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/source.rs`:19 on 2024-07-12 18:17_

The count here was misleading because it incremented the count everytime `SourceText` was cloned

---

_Comment by @MichaReiser on 2024-07-14 09:47_

Closing in favor of https://github.com/astral-sh/ruff/pull/12318

---

_Closed by @MichaReiser on 2024-07-14 09:47_

---

_Branch deleted on 2024-08-12 07:52_

---
