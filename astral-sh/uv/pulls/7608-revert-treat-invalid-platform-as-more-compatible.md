```yaml
number: 7608
title: "Revert \"Treat invalid platform as more compatible than invalid Python (#7556)\""
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/rev
created_at: 2024-09-21T03:30:32Z
updated_at: 2024-09-21T12:36:04Z
url: https://github.com/astral-sh/uv/pull/7608
synced_at: 2026-01-10T12:53:51Z
```

# Revert "Treat invalid platform as more compatible than invalid Python (#7556)"

---

_Pull request opened by @zanieb on 2024-09-21 03:30_

Closes https://github.com/astral-sh/uv/issues/7606

We'll need to dig deeper into the cause here.

---

_Label `bug` added by @zanieb on 2024-09-21 03:30_

---

_Merged by @charliermarsh on 2024-09-21 12:34_

---

_Closed by @charliermarsh on 2024-09-21 12:34_

---

_Branch deleted on 2024-09-21 12:34_

---

_Comment by @charliermarsh on 2024-09-21 12:36_

I can take a look. Strikes me as a sign of some bad behavior somewhere (lol) if changing the priority of two incompatible versions breaks resolutions.

---
