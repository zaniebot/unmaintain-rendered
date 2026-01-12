```yaml
number: 1097
title: Allow specifiying patch Python versions when creating venv on unix
type: pull_request
state: closed
author: zanieb
labels:
  - enhancement
assignees: []
draft: true
base: main
head: zb/venv-patch
created_at: 2024-01-25T16:09:14Z
updated_at: 2024-01-25T20:39:19Z
url: https://github.com/astral-sh/uv/pull/1097
synced_at: 2026-01-12T16:04:25Z
```

# Allow specifiying patch Python versions when creating venv on unix

---

_@zanieb_

Cherry-picked from #1070 where we need virtual environments with a specific patch version.

This is more complex on Windows due to our current version lookup implementation but may be easier to support if we look up versions in the registry.

---

_Renamed from "zb/venv patch" to "Allow specifiying patch Python versions when creating venv on unix" by @zanieb on 2024-01-25 16:09_

---

_Comment by @zanieb on 2024-01-25 16:40_

Okay this is annoying — the right versions aren't available in CI so this will have to wait until we pin versions nicely.

---

_Converted to draft by @zanieb on 2024-01-25 16:40_

---

_Label `enhancement` added by @zanieb on 2024-01-25 16:40_

---

_Comment by @zanieb on 2024-01-25 20:39_

Picked into #1105 instead

---

_Closed by @zanieb on 2024-01-25 20:39_

---
