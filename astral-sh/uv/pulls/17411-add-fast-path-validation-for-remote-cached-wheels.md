```yaml
number: 17411
title: Add fast-path validation for remote-cached wheels
type: pull_request
state: open
author: charliermarsh
labels: []
assignees: []
base: charlie/git-cache
head: charlie/git-cache-val
created_at: 2026-01-12T03:54:02Z
updated_at: 2026-01-12T18:25:07Z
url: https://github.com/astral-sh/uv/pull/17411
synced_at: 2026-01-12T19:14:22Z
```

# Add fast-path validation for remote-cached wheels

---

_@charliermarsh_

_No description provided._

---

_Marked ready for review by @charliermarsh on 2026-01-12 04:04_

---

_Review comment by @konstin on `crates/uv-distribution/src/remote.rs`:439 on 2026-01-12 09:56_

Does that mean we'll silently skip the upload if the server returns a 500 Internal Server Error or has an outage?

---

_Review comment by @konstin on `crates/uv-distribution/src/remote.rs`:444 on 2026-01-12 09:57_

Should we do a JSON response here? That also allows adding more responses in the future.

---

_@konstin reviewed on 2026-01-12 09:59_

---

_@konstin reviewed on 2026-01-12 17:14_

.

---

_@konstin reviewed on 2026-01-12 17:31_

---

_Review comment by @konstin on `crates/uv-distribution/src/remote.rs`:528 on 2026-01-12 17:31_

Can you say some more on why we want to skip instead of error for unsupported wheels?

---

_@charliermarsh reviewed on 2026-01-12 17:34_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/remote.rs`:528 on 2026-01-12 17:34_

This is an operation that happens transparently in the background after wheel building. We don't want to fail the local install of the wheel just because it's not supported in the cache.

---

_@konstin reviewed on 2026-01-12 18:25_

---

_Review comment by @konstin on `crates/uv-distribution/src/remote.rs`:528 on 2026-01-12 18:25_

Can you add a doc comment explaining the outline of it? Otherwise r+

---
