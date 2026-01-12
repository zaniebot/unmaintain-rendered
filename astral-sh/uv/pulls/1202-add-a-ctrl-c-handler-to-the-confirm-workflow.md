```yaml
number: 1202
title: Add a Ctrl+C handler to the confirm workflow
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ctrl
created_at: 2024-01-31T01:44:48Z
updated_at: 2024-01-31T02:08:28Z
url: https://github.com/astral-sh/uv/pull/1202
synced_at: 2026-01-12T16:04:30Z
```

# Add a Ctrl+C handler to the confirm workflow

---

_@charliermarsh_

Fixes an issue whereby exiting the confirmation prompt can lead to your cursor disappearing: https://github.com/console-rs/dialoguer/issues/294.

See: https://github.com/mitsuhiko/rye/blob/b839a2c5b7e648e9e5d836a8bf5f703c9807d615/rye/src/main.rs#L36-L48.


---

_Marked ready for review by @charliermarsh on 2024-01-31 01:44_

---

_Label `bug` added by @charliermarsh on 2024-01-31 01:44_

---

_Merged by @charliermarsh on 2024-01-31 02:08_

---

_Closed by @charliermarsh on 2024-01-31 02:08_

---

_Branch deleted on 2024-01-31 02:08_

---
