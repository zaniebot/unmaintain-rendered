```yaml
number: 16055
title: Ignore origin when comparing installed tools
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/origin
created_at: 2025-09-28T19:05:57Z
updated_at: 2025-09-29T17:23:19Z
url: https://github.com/astral-sh/uv/pull/16055
synced_at: 2026-01-10T06:36:15Z
```

# Ignore origin when comparing installed tools

---

_Pull request opened by @charliermarsh on 2025-09-28 19:05_

## Summary

This field gets dropped when you serialize and deserialize, so we should ignore it when comparing indexes.

Closes https://github.com/astral-sh/uv/issues/16051.


---

_Label `bug` added by @charliermarsh on 2025-09-28 19:06_

---

_Review requested from @zanieb by @charliermarsh on 2025-09-28 19:06_

---

_Review requested from @konstin by @charliermarsh on 2025-09-28 19:06_

---

_Marked ready for review by @charliermarsh on 2025-09-28 19:06_

---

_Review comment by @konstin on `crates/uv/tests/it/tool_install.rs`:4156 on 2025-09-29 09:22_

That's not black

---

_@konstin approved on 2025-09-29 09:22_

---

_Merged by @charliermarsh on 2025-09-29 17:23_

---

_Closed by @charliermarsh on 2025-09-29 17:23_

---

_Branch deleted on 2025-09-29 17:23_

---
