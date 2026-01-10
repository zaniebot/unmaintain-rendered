```yaml
number: 15610
title: Lock the credentials store when reading or writing
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: feature/auth
head: zb/lock-auth
created_at: 2025-08-31T17:05:29Z
updated_at: 2025-09-02T12:59:27Z
url: https://github.com/astral-sh/uv/pull/15610
synced_at: 2026-01-10T06:44:33Z
```

# Lock the credentials store when reading or writing

---

_Pull request opened by @zanieb on 2025-08-31 17:05_

Adds locking of the credentials store for concurrency safety. It's important to hold the lock from read -> write so credentials are not dropped during concurrent writes.

I opted not to attach the lock to the store itself. Instead, I return the lock on read and require it on write to encourage safe use. Maybe attaching the source path to the store struct and adding a `lock(&self)` method would make sense? but then you can forget to take the lock at the right time. The main problem with the interface here is to write a _new_ store you have to take the lock yourself, and you could make a mistake by taking a lock for the wrong path or something. The fix for that would be to introduce a new `CredentialStoreHandle` type or something, but that seems overzealous rn. We also don't eagerly drop the lock on token read, although we could.

---

_Review requested from @charliermarsh by @zanieb on 2025-08-31 17:18_

---

_Marked ready for review by @zanieb on 2025-08-31 20:17_

---

_Merged by @zanieb on 2025-09-02 12:59_

---

_Closed by @zanieb on 2025-09-02 12:59_

---

_Branch deleted on 2025-09-02 12:59_

---
