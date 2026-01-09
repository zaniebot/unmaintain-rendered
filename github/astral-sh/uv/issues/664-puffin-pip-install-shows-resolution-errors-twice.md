---
number: 664
title: "`puffin pip-install` shows resolution errors twice"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-12-15T18:12:54Z
updated_at: 2023-12-15T18:43:25Z
url: https://github.com/astral-sh/uv/issues/664
synced_at: 2026-01-07T13:12:16-06:00
---

# `puffin pip-install` shows resolution errors twice

---

_Issue opened by @charliermarsh on 2023-12-15 18:12_

For example:

```
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of msgraph-core available matching >=1.0.0a2 and msgraph-sdk==1.0.0 depends on msgraph-core>=1.0.0a2, msgraph-sdk==1.0.0 is forbidden.
      And because there is no version of msgraph-sdk available matching <1.0.0 and root depends on msgraph-sdk, version solving failed.
error: Because there is no version of msgraph-core available matching >=1.0.0a2 and msgraph-sdk==1.0.0 depends on msgraph-core>=1.0.0a2, msgraph-sdk==1.0.0 is forbidden.
And because there is no version of msgraph-sdk available matching <1.0.0 and root depends on msgraph-sdk, version solving failed.
```

---

_Label `bug` added by @charliermarsh on 2023-12-15 18:13_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-15 18:13_

---

_Referenced in [astral-sh/uv#665](../../astral-sh/uv/pulls/665.md) on 2023-12-15 18:37_

---

_Closed by @charliermarsh on 2023-12-15 18:43_

---
