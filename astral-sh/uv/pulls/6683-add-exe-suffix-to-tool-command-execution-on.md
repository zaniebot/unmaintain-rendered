```yaml
number: 6683
title: "Add `.exe` suffix to tool command execution on Windows"
type: pull_request
state: open
author: zanieb
labels:
  - bug
assignees: []
base: main
head: zb/use-exe-on-windowss
created_at: 2024-08-27T12:10:16Z
updated_at: 2025-04-14T08:00:32Z
url: https://github.com/astral-sh/uv/pull/6683
synced_at: 2026-01-10T11:10:34Z
```

# Add `.exe` suffix to tool command execution on Windows

---

_Pull request opened by @zanieb on 2024-08-27 12:10_

Should close https://github.com/astral-sh/uv/issues/6682

---

_Label `bug` added by @zanieb on 2024-08-27 12:10_

---

_Converted to draft by @zanieb on 2024-08-27 12:11_

---

_Review requested from @charliermarsh by @zanieb on 2024-08-27 12:58_

---

_Marked ready for review by @zanieb on 2024-08-27 12:58_

---

_Comment by @zanieb on 2024-08-27 12:58_

~Needs validation on Windows and more explicit test coverage.~

@konstin tested this and I added a test with coverage

---

_Comment by @samypr100 on 2024-08-27 19:44_

This wouldn't solve `gimme-aws-creds`'s case right? As there's no exe's in scripts declaration.

---

_Comment by @zanieb on 2024-08-28 14:02_

I presume not yeah.

---
