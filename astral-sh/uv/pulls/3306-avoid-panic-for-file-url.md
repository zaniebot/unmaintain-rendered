```yaml
number: 3306
title: Avoid panic for file url
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/avoid-panic-for-file-url
created_at: 2024-04-29T11:07:58Z
updated_at: 2024-04-29T14:39:17Z
url: https://github.com/astral-sh/uv/pull/3306
synced_at: 2026-01-10T14:37:54Z
```

# Avoid panic for file url

---

_Pull request opened by @konstin on 2024-04-29 11:07_

When using find links with a file url, we shouldn't panic because we can't remove username/password for a host-less url.

See #3262


---

_Label `bug` added by @konstin on 2024-04-29 11:07_

---

_Review requested from @zanieb by @konstin on 2024-04-29 11:15_

---

_Review comment by @zanieb on `crates/cache-key/src/canonical_url.rs`:36 on 2024-04-29 14:34_

Wouldn't we return on line 31 if there's no host? or line 26 if there cannot be a base? Weird.

---

_@zanieb reviewed on 2024-04-29 14:34_

---

_@zanieb approved on 2024-04-29 14:34_

Regardless, panicking here seems weird.

---

_Merged by @konstin on 2024-04-29 14:39_

---

_Closed by @konstin on 2024-04-29 14:39_

---

_Branch deleted on 2024-04-29 14:39_

---
