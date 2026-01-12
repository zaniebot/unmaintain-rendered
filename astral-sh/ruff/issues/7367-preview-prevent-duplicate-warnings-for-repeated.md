```yaml
number: 7367
title: "Preview: Prevent duplicate warnings for repeated selections"
type: issue
state: open
author: zanieb
labels:
  - preview
assignees: []
created_at: 2023-09-13T20:31:57Z
updated_at: 2023-09-13T20:32:02Z
url: https://github.com/astral-sh/ruff/issues/7367
synced_at: 2026-01-12T15:54:47Z
```

# Preview: Prevent duplicate warnings for repeated selections

---

_@zanieb_

When we throw warnings for deprecated selections of nursery rules, we don't check if we have warned before. 

> Because this doesn't use `warn_user_once_by_id`, the warnings will probably show up multiple times if we run this configuration resolution multiple times (which we might in a given invocation -- I can't remember exactly). Is it possible to use `warn_user_once_by_id`?

_Originally posted by @charliermarsh in https://github.com/astral-sh/ruff/pull/7210#discussion_r1323482428_
            

---

_Label `preview` added by @zanieb on 2023-09-13 20:32_

---
