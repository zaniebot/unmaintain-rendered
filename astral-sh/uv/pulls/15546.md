```yaml
number: 15546
title: "Make `cache_index_credentials()` misuse resistant"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/always-cache-index-credentials
created_at: 2025-08-27T09:10:41Z
updated_at: 2025-08-29T15:11:55Z
url: https://github.com/astral-sh/uv/pull/15546
synced_at: 2026-01-10T06:44:33Z
```

# Make `cache_index_credentials()` misuse resistant

---

_Pull request opened by @konstin on 2025-08-27 09:10_

https://github.com/astral-sh/uv/issues/11836#issuecomment-3022735011 was caused by a missing `cache_index_credentials()` call. This call was always preceding a registry client builder. We can improve this situation by caching index credentials in the registry client builder.

---

_Label `internal` added by @konstin on 2025-08-27 09:10_

---

_Review requested from @charliermarsh by @konstin on 2025-08-27 09:10_

---

_@charliermarsh approved on 2025-08-27 11:43_

---

_Merged by @konstin on 2025-08-29 15:11_

---

_Closed by @konstin on 2025-08-29 15:11_

---

_Branch deleted on 2025-08-29 15:11_

---
