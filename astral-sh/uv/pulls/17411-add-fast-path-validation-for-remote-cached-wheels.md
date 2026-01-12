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
updated_at: 2026-01-12T09:59:05Z
url: https://github.com/astral-sh/uv/pull/17411
synced_at: 2026-01-12T11:01:31Z
```

# Add fast-path validation for remote-cached wheels

---

_Pull request opened by @charliermarsh on 2026-01-12 03:54_

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
