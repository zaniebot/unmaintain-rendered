```yaml
number: 12380
title: Add support for pre-releases to Python platform key regex
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/regex-pre
created_at: 2025-03-21T18:51:57Z
updated_at: 2025-03-23T03:19:05Z
url: https://github.com/astral-sh/uv/pull/12380
synced_at: 2026-01-10T11:10:39Z
```

# Add support for pre-releases to Python platform key regex

---

_Pull request opened by @zanieb on 2025-03-21 18:51_

Needed for more test cases on top of #12374 

---

_Marked ready for review by @zanieb on 2025-03-21 18:54_

---

_Converted to draft by @zanieb on 2025-03-21 19:40_

---

_Marked ready for review by @zanieb on 2025-03-21 19:59_

---

_Label `internal` added by @zanieb on 2025-03-21 20:34_

---

_@charliermarsh approved on 2025-03-21 22:36_

Do you want to explicitly match on the allowed pre-release components (rather than accepting any `[a-z]`?

---

_Comment by @zanieb on 2025-03-23 03:02_

> Do you want to explicitly match on the allowed pre-release components (rather than accepting any `[a-z]`?

It's a fair question, it didn't seem worth the effort but it's not hard so, sure.

---

_Merged by @zanieb on 2025-03-23 03:19_

---

_Closed by @zanieb on 2025-03-23 03:19_

---

_Branch deleted on 2025-03-23 03:19_

---
