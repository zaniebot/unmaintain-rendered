```yaml
number: 15554
title: "Add support for credentials in URLs to `uv auth`"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: feature/auth
head: zb/uv-auth-iiii
created_at: 2025-08-27T17:30:11Z
updated_at: 2025-08-28T15:31:29Z
url: https://github.com/astral-sh/uv/pull/15554
synced_at: 2026-01-12T16:11:49Z
```

# Add support for credentials in URLs to `uv auth`

---

_@zanieb_

Allows cases like `uv auth login https://username:password@example.com` for coherence with the rest of our interfaces.

---

_@zanieb reviewed on 2025-08-27 18:06_

---

_Review comment by @zanieb on `crates/uv-auth/src/keyring.rs`:88 on 2025-08-27 18:06_

This is really important. Otherwise, we'd include the username and/or password in the literal service name we're writing to the keychain.

---

_Marked ready for review by @zanieb on 2025-08-28 14:27_

---

_Merged by @zanieb on 2025-08-28 15:31_

---

_Closed by @zanieb on 2025-08-28 15:31_

---

_Branch deleted on 2025-08-28 15:31_

---
