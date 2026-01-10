```yaml
number: 10477
title: "Fix `UV_FIND_LINKS` delimiter to split on commas"
type: pull_request
state: merged
author: foxcroftjn
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-find-links-delimiter
created_at: 2025-01-10T19:51:52Z
updated_at: 2025-01-10T20:04:36Z
url: https://github.com/astral-sh/uv/pull/10477
synced_at: 2026-01-10T11:44:51Z
```

# Fix `UV_FIND_LINKS` delimiter to split on commas

---

_Pull request opened by @foxcroftjn on 2025-01-10 19:51_

#8061 incorrectly claims to change the delimiter for `UV_FIND_LINKS` from spaces to commas. In reality, it prevents `UV_FIND_LINKS` from being split. This commit fixes that.

---

_Comment by @charliermarsh on 2025-01-10 19:54_

Thanks. I think I misremembered and assumed that `,` was the default.

---

_Label `bug` added by @charliermarsh on 2025-01-10 19:54_

---

_Renamed from "Fix UV_FIND_LINKS delimiter" to "Fix `UV_FIND_LINKS` delimiter to split on commas" by @charliermarsh on 2025-01-10 19:54_

---

_Merged by @charliermarsh on 2025-01-10 20:04_

---

_Closed by @charliermarsh on 2025-01-10 20:04_

---
