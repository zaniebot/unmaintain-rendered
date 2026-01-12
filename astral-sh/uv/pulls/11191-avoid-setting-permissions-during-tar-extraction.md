```yaml
number: 11191
title: Avoid setting permissions during tar extraction
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: charlie/perm
created_at: 2025-02-03T19:20:50Z
updated_at: 2025-02-03T19:29:12Z
url: https://github.com/astral-sh/uv/pull/11191
synced_at: 2026-01-12T16:09:43Z
```

# Avoid setting permissions during tar extraction

---

_@charliermarsh_

## Summary

As in our zip operation (and like pip), we want to explicitly avoid setting permissions during unpacking -- apart from setting the executable bit.

This depends on https://github.com/astral-sh/tokio-tar/pull/8.

Closes https://github.com/astral-sh/uv/issues/11188.


---

_Label `bug` added by @charliermarsh on 2025-02-03 19:21_

---

_Label `compatibility` added by @charliermarsh on 2025-02-03 19:21_

---

_Review requested from @Gankra by @charliermarsh on 2025-02-03 19:21_

---

_Review requested from @konstin by @charliermarsh on 2025-02-03 19:21_

---

_Review comment by @charliermarsh on `crates/uv-extract/src/stream.rs`:213 on 2025-02-03 19:21_

None of this matters now that we aren't setting permissions.

---

_@charliermarsh reviewed on 2025-02-03 19:21_

---

_@konstin approved on 2025-02-03 19:23_

nice

---

_Merged by @charliermarsh on 2025-02-03 19:29_

---

_Closed by @charliermarsh on 2025-02-03 19:29_

---

_Branch deleted on 2025-02-03 19:29_

---
