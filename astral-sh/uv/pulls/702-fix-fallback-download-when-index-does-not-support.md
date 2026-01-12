```yaml
number: 702
title: Fix fallback download when index does not support HTTP range requests
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/range-req
created_at: 2023-12-19T17:20:18Z
updated_at: 2023-12-20T10:55:24Z
url: https://github.com/astral-sh/uv/pull/702
synced_at: 2026-01-12T16:04:08Z
```

# Fix fallback download when index does not support HTTP range requests

---

_@zanieb_

Otherwise, when a server does not support HTTP range requests we throw an error instead of downloading without range requests.

---

_Comment by @zanieb on 2023-12-19 19:18_

xref https://github.com/devpi/devpi/issues/1021

---

_Review comment by @charliermarsh on `crates/puffin-client/src/registry_client.rs`:306 on 2023-12-20 02:41_

I'll leave this to @konstin because I'm not sure if this should _also_ include the `CachedClientError::Client` version.

---

_@charliermarsh reviewed on 2023-12-20 02:41_

---

_Review requested from @konstin by @charliermarsh on 2023-12-20 02:41_

---

_@konstin reviewed on 2023-12-20 10:51_

---

_Review comment by @konstin on `crates/puffin-client/src/registry_client.rs`:306 on 2023-12-20 10:51_

We can merge both variant since they carry the same payload

---

_Merged by @konstin on 2023-12-20 10:55_

---

_Closed by @konstin on 2023-12-20 10:55_

---

_Branch deleted on 2023-12-20 10:55_

---
