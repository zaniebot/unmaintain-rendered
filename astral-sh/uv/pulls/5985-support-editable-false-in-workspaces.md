```yaml
number: 5985
title: "Support `editable = false` in workspaces"
type: pull_request
state: closed
author: charliermarsh
labels:
  - enhancement
  - preview
assignees: []
draft: true
base: main
head: charlie/workspace-false
created_at: 2024-08-09T23:33:52Z
updated_at: 2024-08-10T00:59:43Z
url: https://github.com/astral-sh/uv/pull/5985
synced_at: 2026-01-12T16:07:08Z
```

# Support `editable = false` in workspaces

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/5958.

## Test Plan

(Needs tests.)


---

_Label `enhancement` added by @charliermarsh on 2024-08-09 23:34_

---

_Label `preview` added by @charliermarsh on 2024-08-09 23:34_

---

_Comment by @charliermarsh on 2024-08-10 00:06_

Hmm ok, this is actually a bit more complicated. When we collect the URLs, we see the editable member and the non-editable requirement. We then persist the editable member. So when we get to the resolution, we always view that package as editable. We'd need to either (1) let non-editable override editable for workspace members, or (2) somehow make editable part of the requirement rather than part of the package, so that we can support a package existing as both editable and non-editable in a single resolution...


---

_Comment by @charliermarsh on 2024-08-10 00:59_

Will return to this, need to think on it.

---

_Closed by @charliermarsh on 2024-08-10 00:59_

---
