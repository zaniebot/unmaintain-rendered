```yaml
number: 15909
title: Remove tracking of inferred dependency conflicts
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/no-inferred-conflict
created_at: 2025-09-17T12:58:39Z
updated_at: 2025-10-01T15:03:44Z
url: https://github.com/astral-sh/uv/pull/15909
synced_at: 2026-01-12T16:12:00Z
```

# Remove tracking of inferred dependency conflicts

---

_@zanieb_

Alternative to #15884 (see commentary there)

Closes https://github.com/astral-sh/uv/issues/15869

---

_Label `bug` added by @zanieb on 2025-09-17 12:58_

---

_@zanieb reviewed on 2025-09-17 15:51_

---

_Review comment by @zanieb on `crates/uv-pypi-types/src/conflicts.rs`:184 on 2025-09-17 15:51_

This logic can probably be simplified?

---

_Comment by @konstin on 2025-09-25 13:41_

This looks good to me, do we want to go ahead with this?

---

_Marked ready for review by @zanieb on 2025-10-01 14:26_

---

_Comment by @zanieb on 2025-10-01 14:27_

Yeah I was sort of considering doing a larger refactor to clean up the logic, but we can just fix the bug first.

---

_@konstin approved on 2025-10-01 14:40_

---

_Merged by @zanieb on 2025-10-01 15:03_

---

_Closed by @zanieb on 2025-10-01 15:03_

---

_Branch deleted on 2025-10-01 15:03_

---
