```yaml
number: 14716
title: "Ignore \"not found\" errors when removing orphaned registry keys"
type: pull_request
state: closed
author: zanieb
labels:
  - bug
  - windows
assignees: []
base: main
head: zb/not-found
created_at: 2025-07-18T12:19:26Z
updated_at: 2025-07-18T12:35:35Z
url: https://github.com/astral-sh/uv/pull/14716
synced_at: 2026-01-10T06:53:02Z
```

# Ignore "not found" errors when removing orphaned registry keys

---

_Pull request opened by @zanieb on 2025-07-18 12:19_

Closes https://github.com/astral-sh/uv/issues/14714

---

_Label `bug` added by @zanieb on 2025-07-18 12:19_

---

_Label `windows` added by @zanieb on 2025-07-18 12:19_

---

_Comment by @zanieb on 2025-07-18 12:20_

cc @konstin seems like we raced on a fix here. I think we want this regardless? we use it on all the other errors. but it sounds like you have other ideas to make things better :)

---

_Comment by @konstin on 2025-07-18 12:28_

I agree that we want to fix it this way, I have same fix with the err kind check in my branch, plus some extra fixes.

---

_Closed by @zanieb on 2025-07-18 12:35_

---
