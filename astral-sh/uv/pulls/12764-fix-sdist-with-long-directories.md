```yaml
number: 12764
title: Fix sdist with long directories
type: pull_request
state: merged
author: leiserfg
labels:
  - bug
assignees: []
merged: true
base: main
head: deep-build
created_at: 2025-04-08T21:31:19Z
updated_at: 2025-04-09T13:01:22Z
url: https://github.com/astral-sh/uv/pull/12764
synced_at: 2026-01-12T16:10:22Z
```

# Fix sdist with long directories

---

_@leiserfg_

I removed the `set_cksum` as the value of it is replaced inside of `append_data`.

## Summary

This should fix #12762 but I don't know how to test it.




---

_Review requested from @konstin by @charliermarsh on 2025-04-09 02:00_

---

_Label `bug` added by @charliermarsh on 2025-04-09 02:00_

---

_Comment by @konstin on 2025-04-09 09:43_

For testing, we have all build backend tests in crates/uv/tests/it/build_backend.rs, where you can create long paths with rust methods.

---

_@konstin approved on 2025-04-09 12:51_

thanks!

---

_Merged by @konstin on 2025-04-09 13:01_

---

_Closed by @konstin on 2025-04-09 13:01_

---
