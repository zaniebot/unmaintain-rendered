```yaml
number: 11306
title: Include archive bucket version in archive pointers
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: tracking/060
head: charlie/version
created_at: 2025-02-07T02:00:38Z
updated_at: 2025-02-07T22:20:13Z
url: https://github.com/astral-sh/uv/pull/11306
synced_at: 2026-01-12T16:09:46Z
```

# Include archive bucket version in archive pointers

---

_@charliermarsh_

## Summary

We've never bumped the version of this bucket, and we may never do so... But it's still incorrect for us to omit it from these serialized structs in the cache. Specifically, these structs include a pointer into the archive bucket (namely, the ID). But we don't include the bucket version! So, in theory, we could end up pointing to archives that don't match the current bucket version expected in the code.


---

_Review requested from @zanieb by @charliermarsh on 2025-02-07 02:00_

---

_Label `bug` added by @charliermarsh on 2025-02-07 02:00_

---

_Marked ready for review by @charliermarsh on 2025-02-07 02:00_

---

_@zanieb approved on 2025-02-07 02:20_

Well, you know I think it's a good idea.

---

_Added to milestone `v0.6.0` by @charliermarsh on 2025-02-07 02:56_

---

_Review comment by @T-256 on `crates/uv-cache/src/lib.rs`:795 on 2025-02-07 16:03_

```suggestion
            // in `crates/uv/tests/it/cache_prune.rs`.
```

---

_@T-256 reviewed on 2025-02-07 16:03_

---

_Merged by @charliermarsh on 2025-02-07 22:20_

---

_Closed by @charliermarsh on 2025-02-07 22:20_

---

_Branch deleted on 2025-02-07 22:20_

---
