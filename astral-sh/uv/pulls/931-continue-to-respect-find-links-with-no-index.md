```yaml
number: 931
title: "Continue to respect `--find-links` with `--no-index`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/warn
created_at: 2024-01-15T16:07:34Z
updated_at: 2024-01-15T16:19:28Z
url: https://github.com/astral-sh/uv/pull/931
synced_at: 2026-01-10T15:39:02Z
```

# Continue to respect `--find-links` with `--no-index`

---

_Pull request opened by @charliermarsh on 2024-01-15 16:07_

Like `pip`, we should allow `--find-links` with `--no-index`.

---

_Review requested from @konstin by @charliermarsh on 2024-01-15 16:07_

---

_Label `bug` added by @charliermarsh on 2024-01-15 16:08_

---

_@konstin approved on 2024-01-15 16:08_

---

_Comment by @charliermarsh on 2024-01-15 16:09_

Adding test...

---

_@charliermarsh reviewed on 2024-01-15 16:18_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/registry_client.rs`:270 on 2024-01-15 16:18_

IMO these should only be enforced on `simple` where we're actually querying the registry.

---

_@charliermarsh reviewed on 2024-01-15 16:18_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/registry_client.rs`:270 on 2024-01-15 16:18_

(If we want to prevent network requests, we should add a `--offline` flag.)

---

_Merged by @charliermarsh on 2024-01-15 16:19_

---

_Closed by @charliermarsh on 2024-01-15 16:19_

---

_Branch deleted on 2024-01-15 16:19_

---
