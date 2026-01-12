```yaml
number: 15879
title: Store native credentials for realms with the https scheme stripped
type: pull_request
state: merged
author: zanieb
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: zb/native-realm-fix
created_at: 2025-09-15T16:39:08Z
updated_at: 2025-09-15T18:47:35Z
url: https://github.com/astral-sh/uv/pull/15879
synced_at: 2026-01-12T16:12:00Z
```

# Store native credentials for realms with the https scheme stripped

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/15818

Unfortunately, this is how we perform lookups. We could also change the lookup to include the scheme. I expect all of this to change in the future anyway, as I want to redesign the storage model for native credentials.



---

_Label `bug` added by @zanieb on 2025-09-15 16:39_

---

_Label `preview` added by @zanieb on 2025-09-15 16:39_

---

_Label `bug` added by @zanieb on 2025-09-15 16:39_

---

_Label `preview` added by @zanieb on 2025-09-15 16:39_

---

_Marked ready for review by @zanieb on 2025-09-15 16:41_

---

_@zanieb reviewed on 2025-09-15 18:46_

---

_Review comment by @zanieb on `crates/uv-auth/src/keyring.rs`:157 on 2025-09-15 18:46_

This repetition is questionable, but I'll refactor this in a subsequent change this week.

---

_Merged by @zanieb on 2025-09-15 18:47_

---

_Closed by @zanieb on 2025-09-15 18:47_

---

_Branch deleted on 2025-09-15 18:47_

---
