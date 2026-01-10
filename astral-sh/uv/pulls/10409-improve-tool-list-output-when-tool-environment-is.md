```yaml
number: 10409
title: Improve tool list output when tool environment is broken
type: pull_request
state: merged
author: blueraft
labels:
  - bug
assignees: []
merged: true
base: main
head: broken-env-tool-list
created_at: 2025-01-08T20:13:46Z
updated_at: 2025-01-10T08:16:49Z
url: https://github.com/astral-sh/uv/pull/10409
synced_at: 2026-01-10T11:44:47Z
```

# Improve tool list output when tool environment is broken

---

_Pull request opened by @blueraft on 2025-01-08 20:13_

## Summary

Closes #9579 

## Test Plan

`cargo test`


<img width="1110" alt="Screenshot 2025-01-08 at 21 13 11" src="https://github.com/user-attachments/assets/8937c14e-c594-470f-a8f2-77ac7167794f" />


---

_Assigned to @zanieb by @zanieb on 2025-01-08 20:58_

---

_@zanieb reviewed on 2025-01-09 00:31_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:57 on 2025-01-09 00:31_

I think we want to suggest reinstall instead of uninstall, right? Does it work?

---

_Review comment by @blueraft on `crates/uv/src/commands/tool/list.rs`:57 on 2025-01-09 10:29_

yeah that seems better. it worked for me! 

---

_@blueraft reviewed on 2025-01-09 10:29_

---

_Label `bug` added by @zanieb on 2025-01-09 14:31_

---

_@zanieb approved on 2025-01-09 14:31_

Thanks!

---

_Merged by @zanieb on 2025-01-09 21:48_

---

_Closed by @zanieb on 2025-01-09 21:48_

---

_Branch deleted on 2025-01-10 08:16_

---
