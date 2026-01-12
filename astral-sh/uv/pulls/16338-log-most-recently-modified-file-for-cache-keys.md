```yaml
number: 16338
title: Log most recently modified file for cache-keys
type: pull_request
state: merged
author: konstin
labels:
  - tracing
assignees: []
merged: true
base: main
head: konsti/log-last-modified-timestamp
created_at: 2025-10-17T08:59:35Z
updated_at: 2025-11-02T18:52:09Z
url: https://github.com/astral-sh/uv/pull/16338
synced_at: 2026-01-12T16:12:13Z
```

# Log most recently modified file for cache-keys

---

_@konstin_

For https://github.com/astral-sh/uv/issues/16336. We previously weren't telling the user which file is responsible for rebuilding.


---

_Label `tracing` added by @konstin on 2025-10-17 08:59_

---

_@konstin reviewed on 2025-10-17 09:00_

---

_Review comment by @konstin on `crates/uv-cache-info/src/cache_info.rs`:136 on 2025-10-17 09:00_

The assumption is that this is called rarely while globs have many files, so we convert and allocate here.

---

_Review requested from @zanieb by @konstin on 2025-10-20 11:58_

---

_@charliermarsh approved on 2025-11-02 18:41_

---

_Merged by @charliermarsh on 2025-11-02 18:52_

---

_Closed by @charliermarsh on 2025-11-02 18:52_

---

_Branch deleted on 2025-11-02 18:52_

---
