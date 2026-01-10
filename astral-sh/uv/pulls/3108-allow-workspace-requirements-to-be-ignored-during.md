```yaml
number: 3108
title: "Allow workspace requirements to be ignored during `uv run`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - cli
assignees: []
merged: true
base: main
head: zb/uv-run-no-workspace
created_at: 2024-04-17T22:00:26Z
updated_at: 2024-04-19T14:52:06Z
url: https://github.com/astral-sh/uv/pull/3108
synced_at: 2026-01-10T14:43:31Z
```

# Allow workspace requirements to be ignored during `uv run`

---

_Pull request opened by @zanieb on 2024-04-17 22:00_

_No description provided._

---

_Label `internal` added by @zanieb on 2024-04-17 22:00_

---

_Label `cli` added by @zanieb on 2024-04-17 22:00_

---

_Marked ready for review by @zanieb on 2024-04-17 22:26_

---

_@charliermarsh approved on 2024-04-18 00:35_

Mixed on this, do we know we'll need it?

---

_Comment by @zanieb on 2024-04-18 01:18_

Yeah if I want to invoke some random package from a project directory it's really annoying to build the project e.g. in uv it starts a maturin build. We could remove it later if it feels unnecessary. 

---

_Merged by @zanieb on 2024-04-19 14:52_

---

_Closed by @zanieb on 2024-04-19 14:52_

---

_Branch deleted on 2024-04-19 14:52_

---
