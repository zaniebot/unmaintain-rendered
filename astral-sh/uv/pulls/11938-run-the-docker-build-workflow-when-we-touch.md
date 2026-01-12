```yaml
number: 11938
title: Run the Docker build workflow when we touch project or toolchain metadata
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/docker-workfow
created_at: 2025-03-03T23:17:02Z
updated_at: 2025-03-04T14:36:22Z
url: https://github.com/astral-sh/uv/pull/11938
synced_at: 2026-01-12T16:10:04Z
```

# Run the Docker build workflow when we touch project or toolchain metadata

---

_@zanieb_

I noticed that https://github.com/astral-sh/uv/pull/11936 did not run the Docker builds, nor did #11934 

We should run these when the relevant files change so there aren't surprises at release time!

Updates the `build-binaries` workflow to include toolchain version changes and `.cargo/config.toml` changes too.

---

_Label `testing` added by @zanieb on 2025-03-03 23:17_

---

_Review requested from @charliermarsh by @zanieb on 2025-03-03 23:17_

---

_@zanieb reviewed on 2025-03-03 23:18_

---

_Review comment by @zanieb on `.github/workflows/build-binaries.yml`:17 on 2025-03-03 23:18_

Updated the comment for consistency

---

_@zanieb reviewed on 2025-03-03 23:18_

---

_Review comment by @zanieb on `.github/workflows/build-binaries.yml`:21 on 2025-03-03 23:18_

This is new

---

_@zanieb reviewed on 2025-03-03 23:18_

---

_Review comment by @zanieb on `.github/workflows/build-binaries.yml`:24 on 2025-03-03 23:18_

This is new

---

_Review requested from @jtfmumm by @zanieb on 2025-03-04 14:23_

---

_Review requested from @konstin by @zanieb on 2025-03-04 14:23_

---

_@konstin approved on 2025-03-04 14:28_

---

_Merged by @zanieb on 2025-03-04 14:36_

---

_Closed by @zanieb on 2025-03-04 14:36_

---

_Branch deleted on 2025-03-04 14:36_

---
