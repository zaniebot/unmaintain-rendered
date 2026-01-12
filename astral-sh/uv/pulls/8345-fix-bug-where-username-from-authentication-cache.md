```yaml
number: 8345
title: Fix bug where username from authentication cache could be ignored
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-auth-username
created_at: 2024-10-18T18:33:10Z
updated_at: 2024-10-18T23:45:33Z
url: https://github.com/astral-sh/uv/pull/8345
synced_at: 2026-01-12T16:08:16Z
```

# Fix bug where username from authentication cache could be ignored

---

_@zanieb_

Basically, if username-only authentication came from the _cache_ instead of being present on the _request URL_ to start, we'd end up ignoring it during password lookups which breaks keyring.

Includes some cosmetic changes to the logging and commentary in the middleware, because I was confused when reading the code and logs.

---

_Label `bug` added by @zanieb on 2024-10-18 18:33_

---

_@zanieb reviewed on 2024-10-18 18:34_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:253 on 2024-10-18 18:34_

This one line change fixes the bug. If we had a username in the URL cache but no password in the realm cache we'd drop the password here.

---

_Marked ready for review by @zanieb on 2024-10-18 18:34_

---

_Review requested from @charliermarsh by @zanieb on 2024-10-18 18:35_

---

_@charliermarsh approved on 2024-10-18 18:37_

Thanks for picking this up!

---

_Merged by @zanieb on 2024-10-18 23:45_

---

_Closed by @zanieb on 2024-10-18 23:45_

---

_Branch deleted on 2024-10-18 23:45_

---
