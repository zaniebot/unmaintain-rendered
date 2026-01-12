```yaml
number: 13656
title: Stack traces from Windows CI crashes
type: pull_request
state: merged
author: konstin
labels:
  - internal
  - windows
assignees: []
merged: true
base: main
head: konsti/stack-traces-from-windows-CI
created_at: 2025-05-26T12:21:15Z
updated_at: 2025-05-26T20:22:43Z
url: https://github.com/astral-sh/uv/pull/13656
synced_at: 2026-01-12T16:10:47Z
```

# Stack traces from Windows CI crashes

---

_@konstin_

We regularly have Windows CI crashing with `exit_code: -1073741819`, a recent example is <https://github.com/astral-sh/uv/actions/runs/15244692977/job/42869570968?pr=13650>. This code apparently means Access Violation, akin to a Segmentation Fault. Lacking local reproducibility (at least I never saw this on my Windows machine), I generated workflow steps that will hopefully give us a stack trace (and only fail an already failed job when they are actually bogus; I didn't find any good references).


---

_Review requested from @Gankra by @konstin on 2025-05-26 12:21_

---

_Label `internal` added by @konstin on 2025-05-26 12:21_

---

_Label `windows` added by @konstin on 2025-05-26 12:21_

---

_Comment by @konstin on 2025-05-26 12:29_

Another recent example: https://github.com/astral-sh/uv/actions/runs/15244692977/job/42869570968?pr=13650

---

_@charliermarsh approved on 2025-05-26 16:41_

---

_@Gankra approved on 2025-05-26 17:53_

Neat! I feel like i should recommend rust-minidump for this, but this is probably fine haha.

---

_Merged by @konstin on 2025-05-26 20:22_

---

_Closed by @konstin on 2025-05-26 20:22_

---

_Branch deleted on 2025-05-26 20:22_

---
