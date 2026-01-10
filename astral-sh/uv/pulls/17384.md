```yaml
number: 17384
title: Recognize armv8l as an alias for armv7l in platform tag parsing
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: claude/fix-platform-compatibility-1HdcT
created_at: 2026-01-09T17:51:27Z
updated_at: 2026-01-09T18:29:23Z
url: https://github.com/astral-sh/uv/pull/17384
synced_at: 2026-01-10T05:49:14Z
```

# Recognize armv8l as an alias for armv7l in platform tag parsing

---

_Pull request opened by @zanieb on 2026-01-09 17:51_

Closes #17375

It looks like this was missed in https://github.com/astral-sh/uv/pull/8881 and now that we fail if wheel tags do not match the target environment (see https://github.com/astral-sh/uv/pull/16074), we do not allow armv8l wheels to installed in environments where the interpreter reports armv7l.

---

_Label `bug` added by @zanieb on 2026-01-09 18:18_

---

_Marked ready for review by @zanieb on 2026-01-09 18:21_

---

_@charliermarsh approved on 2026-01-09 18:23_

---

_Merged by @zanieb on 2026-01-09 18:29_

---

_Closed by @zanieb on 2026-01-09 18:29_

---
