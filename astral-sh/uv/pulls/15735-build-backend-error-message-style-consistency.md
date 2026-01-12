```yaml
number: 15735
title: Build backend error message style consistency
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/build-backend-style-consistency
created_at: 2025-09-08T13:39:09Z
updated_at: 2025-09-09T17:34:57Z
url: https://github.com/astral-sh/uv/pull/15735
synced_at: 2026-01-12T16:11:54Z
```

# Build backend error message style consistency

---

_@konstin_

Consistently omit backticks after a colon in build backend messages, following https://github.com/astral-sh/uv/pull/15733#discussion_r2330156783.

There's still 74 matches for `: {}"` and 183 matches for `: {[^{]*}"`, but this PR clears all matches in the build backend.

---

_Review requested from @zanieb by @konstin on 2025-09-08 13:39_

---

_Label `error messages` added by @konstin on 2025-09-08 13:39_

---

_@zanieb approved on 2025-09-08 13:41_

---

_Merged by @konstin on 2025-09-09 17:34_

---

_Closed by @konstin on 2025-09-09 17:34_

---

_Branch deleted on 2025-09-09 17:34_

---
