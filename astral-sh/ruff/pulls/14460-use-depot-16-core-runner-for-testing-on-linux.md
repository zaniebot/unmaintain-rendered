```yaml
number: 14460
title: Use Depot 16-core runner for testing on Linux
type: pull_request
state: merged
author: zanieb
labels:
  - ci
assignees: []
merged: true
base: main
head: zb/ci-depot-linux
created_at: 2024-11-19T14:36:05Z
updated_at: 2024-11-20T10:56:47Z
url: https://github.com/astral-sh/ruff/pull/14460
synced_at: 2026-01-12T15:55:47Z
```

# Use Depot 16-core runner for testing on Linux

---

_@zanieb_

Reduces Linux test CI to 1m 40s (16 core) or 2m 56s (8 core)  to  from 4m 25s. Times are approximate, as runner performance is pretty variable.

In uv, we use the 16 core runners.

---

_Label `internal` added by @zanieb on 2024-11-19 14:36_

---

_Label `internal` removed by @MichaReiser on 2024-11-19 14:41_

---

_Label `ci` added by @MichaReiser on 2024-11-19 14:41_

---

_Comment by @github-actions[bot] on 2024-11-19 14:55_

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

_Renamed from "Use Depot 8-core runner for testing on Linux" to "Use Depot 16-core runner for testing on Linux" by @zanieb on 2024-11-19 17:43_

---

_Marked ready for review by @zanieb on 2024-11-19 17:43_

---

_@AlexWaygood approved on 2024-11-19 22:52_

---

_Merged by @zanieb on 2024-11-20 00:00_

---

_Closed by @zanieb on 2024-11-20 00:00_

---

_Branch deleted on 2024-11-20 00:00_

---

_Comment by @AlexWaygood on 2024-11-20 10:50_

Hmm, I'm now seeing long queue times on most Ruff PRs before the linux job starts -- the Windows job is now finishing before the Linux job! Is it possible we're hitting some sort of concurrency limit here?

---

_Comment by @AlexWaygood on 2024-11-20 10:56_

This is what I see on the `ruff-0.8` branch right now, for example:

![image](https://github.com/user-attachments/assets/0246aad1-47ff-4c01-99ab-5ca648b487fb)


---
