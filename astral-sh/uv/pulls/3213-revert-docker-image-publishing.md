```yaml
number: 3213
title: Revert Docker image publishing
type: pull_request
state: closed
author: zanieb
labels:
  - releases
assignees: []
base: main
head: zb/revert-docker
created_at: 2024-04-23T15:12:08Z
updated_at: 2024-04-23T15:43:09Z
url: https://github.com/astral-sh/uv/pull/3213
synced_at: 2026-01-10T14:43:32Z
```

# Revert Docker image publishing

---

_Pull request opened by @zanieb on 2024-04-23 15:12_

This reverts commit 1bf9d879ab2a3302b1b30d8387633157f884d27d from #3155 and ccf4a85f89b9eacbc2a5261bb32059a377349703 from #3195.

This failed again https://github.com/astral-sh/uv/actions/runs/8802514365/job/24158431085 despite #3195 

We need to verify this works again before we add it to the release pipeline — I need to make a release so reverting instead of fixing.


---

_Label `release` added by @zanieb on 2024-04-23 15:12_

---

_Renamed from "Revert "Re-add Docker image to release pipeline (#3155)"" to "Revert Docker image publishing" by @zanieb on 2024-04-23 15:18_

---

_Comment by @zanieb on 2024-04-23 15:43_

Resolved by fixing permissions in GitHub itself.

---

_Closed by @zanieb on 2024-04-23 15:43_

---
