```yaml
number: 5803
title: Redact registry credentials in lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: charlie/redact
created_at: 2024-08-05T23:29:17Z
updated_at: 2024-08-06T02:54:20Z
url: https://github.com/astral-sh/uv/pull/5803
synced_at: 2026-01-12T16:07:02Z
```

# Redact registry credentials in lockfile

---

_@charliermarsh_

## Summary

Okay, I tested this against...

- Our public "private" proxy
- Fury
- AWS CodeArtifact
- Azure Artifacts

It took a long time.

All of them work as expected with this approach: we omit the credentials from the lockfile, then wire them back up when the index URL is provided during subsequent operations.

Closes https://github.com/astral-sh/uv/issues/5119.


---

_Label `enhancement` added by @charliermarsh on 2024-08-05 23:29_

---

_Label `preview` added by @charliermarsh on 2024-08-05 23:29_

---

_Review requested from @zanieb by @charliermarsh on 2024-08-05 23:29_

---

_Review comment by @zanieb on `crates/uv/tests/lock.rs`:4454 on 2024-08-06 00:20_

What happens if you don't provide an `index-url`?

---

_@zanieb reviewed on 2024-08-06 00:20_

---

_@zanieb approved on 2024-08-06 00:20_

---

_Merged by @charliermarsh on 2024-08-06 02:54_

---

_Closed by @charliermarsh on 2024-08-06 02:54_

---

_Branch deleted on 2024-08-06 02:54_

---
