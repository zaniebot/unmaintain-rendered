```yaml
number: 15121
title: Include all site packages directories in ephemeral environment overlays
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-site-package
created_at: 2025-08-07T01:14:21Z
updated_at: 2025-08-08T18:49:22Z
url: https://github.com/astral-sh/uv/pull/15121
synced_at: 2026-01-10T06:44:33Z
```

# Include all site packages directories in ephemeral environment overlays

---

_Pull request opened by @zanieb on 2025-08-07 01:14_

Related to https://github.com/astral-sh/uv/issues/15113

The case in the linked issue is that we perhaps should not be allowing `uv run --with` with system interpreters at all. I think we can consider that, but the issue highlighted that `uv run --with` for a system interpreter is broken if the base interpreter has custom site packages. This generalizes beyond system interpreters so we should probably fix our overlays.


---

_Label `bug` added by @zanieb on 2025-08-07 01:14_

---

_Comment by @zanieb on 2025-08-07 19:46_

This is failing because we only use purelib and platlib to determine site packages but we need to actually read all of the site directories.

---

_Marked ready for review by @zanieb on 2025-08-08 15:47_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:1938 on 2025-08-08 15:48_

This would fail without this change

---

_@zanieb reviewed on 2025-08-08 15:48_

---

_Comment by @zanieb on 2025-08-08 16:01_

Interesting, now I broke something else. Will investigate that.

---

_@geofft approved on 2025-08-08 18:44_

---

_Merged by @zanieb on 2025-08-08 18:49_

---

_Closed by @zanieb on 2025-08-08 18:49_

---

_Branch deleted on 2025-08-08 18:49_

---
