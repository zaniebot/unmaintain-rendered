```yaml
number: 16768
title: Use explicit credentials cache instead of global static
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/dont-use-a-global-mutable-credential-cache
created_at: 2025-11-18T16:00:02Z
updated_at: 2025-12-03T13:51:28Z
url: https://github.com/astral-sh/uv/pull/16768
synced_at: 2026-01-12T16:12:25Z
```

# Use explicit credentials cache instead of global static

---

_@konstin_

Fixes https://github.com/astral-sh/uv/issues/16447

Passing this around explicitly uncovers some behaviors where we pass e.g. the credentials store to reading the lockfile. The changes in this PR should preserve the existing behavior for now, they only make the locations we read from more explicit.

Labeling this PR as "Enhancement" instead of "Internal" in case this changes behavior when it shouldn't have.

---

_Renamed from "Don't use a global static as credentials cache" to "Use explicit credentials cache instead of global static" by @konstin on 2025-12-03 12:45_

---

_Label `enhancement` added by @konstin on 2025-12-03 12:45_

---

_Marked ready for review by @konstin on 2025-12-03 12:47_

---

_@zanieb approved on 2025-12-03 12:59_

---

_Merged by @konstin on 2025-12-03 13:51_

---

_Closed by @konstin on 2025-12-03 13:51_

---

_Branch deleted on 2025-12-03 13:51_

---
