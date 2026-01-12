```yaml
number: 1152
title: Avoid unnecessary permissions changes for copy paths
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/perms
created_at: 2024-01-28T03:07:17Z
updated_at: 2024-01-28T03:12:02Z
url: https://github.com/astral-sh/uv/pull/1152
synced_at: 2026-01-12T16:04:28Z
```

# Avoid unnecessary permissions changes for copy paths

---

_@charliermarsh_

In Rust, `fs::copy` automatically preserves permissions (see: https://doc.rust-lang.org/std/fs/fn.copy.html).

Elsewhere, when copying from the zip archive out to the cache, we can set permissions during file creation, rather than as a separate call.

Both of these should be slightly more efficient.

---

_Label `enhancement` added by @charliermarsh on 2024-01-28 03:07_

---

_Merged by @charliermarsh on 2024-01-28 03:11_

---

_Closed by @charliermarsh on 2024-01-28 03:11_

---

_Branch deleted on 2024-01-28 03:11_

---

_Comment by @charliermarsh on 2024-01-28 03:12_

Also adds tests for permissions preservation.

---
