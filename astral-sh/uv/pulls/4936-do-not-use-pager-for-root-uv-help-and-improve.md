```yaml
number: 4936
title: "Do not use pager for root `uv help` and improve after help hint"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/help-root
created_at: 2024-07-09T19:14:30Z
updated_at: 2024-07-09T19:38:30Z
url: https://github.com/astral-sh/uv/pull/4936
synced_at: 2026-01-12T16:06:32Z
```

# Do not use pager for root `uv help` and improve after help hint

---

_@zanieb_

Adds a nice hint at the bottom of `uv help` output indicating how to get more details about a specific command, roughly matching Cargo's interface.

We use the short help and skip the pager for the root `uv help` since it's intended to be a landing page for the help interface more than an in-depth display. This also matches Cargo, though I like that they have the global options above the commands and I've not changed that here.

---

_Label `cli` added by @zanieb on 2024-07-09 19:14_

---

_Marked ready for review by @zanieb on 2024-07-09 19:18_

---

_@charliermarsh approved on 2024-07-09 19:26_

---

_Merged by @zanieb on 2024-07-09 19:38_

---

_Closed by @zanieb on 2024-07-09 19:38_

---

_Branch deleted on 2024-07-09 19:38_

---
