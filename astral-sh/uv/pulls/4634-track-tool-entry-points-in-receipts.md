```yaml
number: 4634
title: Track tool entry points in receipts
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/tool-receipt-entry-points
created_at: 2024-06-28T19:31:06Z
updated_at: 2024-06-29T03:45:42Z
url: https://github.com/astral-sh/uv/pull/4634
synced_at: 2026-01-12T16:06:21Z
```

# Track tool entry points in receipts

---

_@zanieb_

We need this to power uninstallations! 

The latter two commits were reviewed in:

- #4637 
- #4638 

Note this is a breaking change for existing tool installations, but it's in preview and very new. In the future, we'll need a clear upgrade path for tool receipt changes.

---

_Label `preview` added by @zanieb on 2024-06-28 19:31_

---

_@zanieb reviewed on 2024-06-28 20:09_

---

_Review comment by @zanieb on `crates/uv/tests/tool_install.rs`:83 on 2024-06-28 20:09_

I'm still honestly on the fence if we should use a mapping with an inline table here instead.

---

_Marked ready for review by @zanieb on 2024-06-28 20:09_

---

_Review requested from @charliermarsh by @zanieb on 2024-06-28 20:27_

---

_Review requested from @konstin by @zanieb on 2024-06-28 20:27_

---

_@charliermarsh approved on 2024-06-28 21:05_

Nice.

---

_Merged by @zanieb on 2024-06-29 03:45_

---

_Closed by @zanieb on 2024-06-29 03:45_

---

_Branch deleted on 2024-06-29 03:45_

---
