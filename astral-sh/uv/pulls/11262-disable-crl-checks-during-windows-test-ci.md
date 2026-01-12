```yaml
number: 11262
title: Disable CRL checks during Windows test CI
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: zb/windows-no-revoke
created_at: 2025-02-05T20:48:59Z
updated_at: 2025-02-10T23:49:40Z
url: https://github.com/astral-sh/uv/pull/11262
synced_at: 2026-01-12T16:09:45Z
```

# Disable CRL checks during Windows test CI

---

_@zanieb_

Resolving

```
curl: (35) schannel: next InitializeSecurityContext failed: CRYPT_E_REVOCATION_OFFLINE (0x80092013) 
- The revocation function was unable to check revocation because the revocation server was offline.
```

---

_Renamed from "Fix Windows CI" to "Disable CRL checks during Windows test CI" by @zanieb on 2025-02-05 20:58_

---

_Marked ready for review by @zanieb on 2025-02-05 20:58_

---

_Comment by @zanieb on 2025-02-05 21:03_

Per https://github.com/astral-sh/uv/actions/runs/13166460293/job/36747724120?pr=11262 it's unclear if this worked or if it just spuriously succeeded elsewhere 😭 

---

_Comment by @zanieb on 2025-02-05 21:09_

Trying a SChannel specific variable

---

_Converted to draft by @zanieb on 2025-02-05 22:18_

---

_Comment by @zanieb on 2025-02-05 22:19_

Alas this did not work.

---

_Closed by @zanieb on 2025-02-10 23:49_

---
