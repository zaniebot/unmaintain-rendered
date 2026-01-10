```yaml
number: 8017
title: "`test_prepare_metadata` hash changes on uv version bump"
type: issue
state: closed
author: zanieb
labels:
  - internal
  - testing
assignees: []
created_at: 2024-10-08T19:49:33Z
updated_at: 2024-10-15T09:27:34Z
url: https://github.com/astral-sh/uv/issues/8017
synced_at: 2026-01-10T04:45:10Z
```

# `test_prepare_metadata` hash changes on uv version bump

---

_Issue opened by @zanieb on 2024-10-08 19:49_

See failure at https://github.com/astral-sh/uv/actions/runs/11242534493/job/31256449318?pr=8016 and fix at https://github.com/astral-sh/uv/pull/8016/commits/6a7c2962143be7b238e57d96554c5572cec186bd

I did not verify the cause of this, I figure @konstin will know.

---

_Assigned to @konstin by @zanieb on 2024-10-08 19:49_

---

_Label `internal` added by @zanieb on 2024-10-08 19:49_

---

_Label `testing` added by @zanieb on 2024-10-08 19:49_

---

_Comment by @konstin on 2024-10-08 20:33_

We should redact that hash, it pulls in the uv version number because it's listed as generator in the WHEEL file.

---

_Closed by @konstin on 2024-10-15 09:27_

---
