```yaml
number: 2464
title: Add unit test coverage for keyring authentication
type: issue
state: closed
author: zanieb
labels:
  - help wanted
  - testing
  - registry
assignees: []
created_at: 2024-03-14T18:44:45Z
updated_at: 2024-04-17T17:23:27Z
url: https://github.com/astral-sh/uv/issues/2464
synced_at: 2026-01-10T05:40:32Z
```

# Add unit test coverage for keyring authentication

---

_Issue opened by @zanieb on 2024-03-14 18:44_

Similar to #2447 — but we should use a keyring provider instead of in-url authentication.

We'll probably need to install and configure keyring to temporarily use a password (bypassing the system keychain). We don't want this to affect the developer's machine.

---

_Label `testing` added by @zanieb on 2024-03-14 18:44_

---

_Label `help wanted` added by @zanieb on 2024-03-14 18:45_

---

_Label `registry` added by @zanieb on 2024-03-14 18:45_

---

_Comment by @zanieb on 2024-04-16 16:50_

We have more test coverage in https://github.com/astral-sh/uv/pull/2976 now but still not end-to-end.

---

_Closed by @zanieb on 2024-04-17 17:23_

---
