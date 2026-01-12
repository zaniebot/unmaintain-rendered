```yaml
number: 14235
title: "Don't log GitHub fast path usage if it's cached"
type: pull_request
state: merged
author: konstin
labels:
  - tracing
assignees: []
merged: true
base: main
head: konsti/fast-path-logging
created_at: 2025-06-24T10:31:08Z
updated_at: 2025-06-24T15:53:11Z
url: https://github.com/astral-sh/uv/pull/14235
synced_at: 2026-01-12T16:11:06Z
```

# Don't log GitHub fast path usage if it's cached

---

_@konstin_

Don't log that we resolved a reference through the GitHub fast path if we didn't use GitHub at all but used the cached revision. This avoids stating that the fast path works when it's blocked due to unrelated reasons (e.g. rate limits).


---

_Review requested from @oconnor663 by @konstin on 2025-06-24 10:31_

---

_Label `tracing` added by @konstin on 2025-06-24 10:31_

---

_@charliermarsh approved on 2025-06-24 13:48_

---

_Merged by @charliermarsh on 2025-06-24 15:53_

---

_Closed by @charliermarsh on 2025-06-24 15:53_

---

_Branch deleted on 2025-06-24 15:53_

---
