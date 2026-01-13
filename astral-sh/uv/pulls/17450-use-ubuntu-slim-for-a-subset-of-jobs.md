```yaml
number: 17450
title: "Use `ubuntu-slim` for a subset of jobs"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: claude/slim-github-runner-CTFPE
created_at: 2026-01-13T22:46:57Z
updated_at: 2026-01-13T23:16:31Z
url: https://github.com/astral-sh/uv/pull/17450
synced_at: 2026-01-13T23:35:46Z
```

# Use `ubuntu-slim` for a subset of jobs

---

_@zanieb_

_No description provided._

---

_Label `internal` added by @zanieb on 2026-01-13 22:47_

---

_Comment by @zanieb on 2026-01-13 22:58_

We can't use this for zizmor, because it needs Docker.

We can't use this for cargo fmt or shear because they need rustup / cargo.

---

_Marked ready for review by @zanieb on 2026-01-13 23:00_

---

_Comment by @woodruffw on 2026-01-13 23:11_

> We can't use this for zizmor, because it needs Docker.

That's a good reason for me to de-Docker it 🙂 

(We could also probably get away with using `uvx zizmor ...`, honestly. The primary difference between that and the `zizmor-action` is that the later does the CodeQL scaffolding setup internally.)

---

_@woodruffw approved on 2026-01-13 23:11_

---

_Merged by @zanieb on 2026-01-13 23:16_

---

_Closed by @zanieb on 2026-01-13 23:16_

---
