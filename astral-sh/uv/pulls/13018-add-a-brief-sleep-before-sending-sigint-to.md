```yaml
number: 13018
title: Add a brief sleep before sending SIGINT to children
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/sleep-int
created_at: 2025-04-21T16:01:15Z
updated_at: 2025-04-21T19:47:41Z
url: https://github.com/astral-sh/uv/pull/13018
synced_at: 2026-01-10T11:10:40Z
```

# Add a brief sleep before sending SIGINT to children

---

_Pull request opened by @zanieb on 2025-04-21 16:01_

In an attempt to avoid interrupting the child if it is in the process of exiting.

This resolves the issue with marimo reported in https://github.com/astral-sh/uv/issues/12108#issuecomment-2745933178 and https://github.com/marimo-team/marimo/issues/4224

---

_Label `enhancement` added by @zanieb on 2025-04-21 16:01_

---

_Comment by @akshayka on 2025-04-21 16:13_

Thank you!

---

_Review requested from @charliermarsh by @zanieb on 2025-04-21 17:44_

---

_@charliermarsh approved on 2025-04-21 19:34_

---

_Merged by @zanieb on 2025-04-21 19:47_

---

_Closed by @zanieb on 2025-04-21 19:47_

---

_Branch deleted on 2025-04-21 19:47_

---
