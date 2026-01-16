```yaml
number: 17503
title: "Fix infinite loop when `SSL_CERT_FILE` is a directory"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: claude/investigate-issues-tN0fc
created_at: 2026-01-15T22:00:23Z
updated_at: 2026-01-16T13:12:55Z
url: https://github.com/astral-sh/uv/pull/17503
synced_at: 2026-01-16T13:57:23Z
```

# Fix infinite loop when `SSL_CERT_FILE` is a directory

---

_@zanieb_

Closes #17494

See https://github.com/rustls/pki-types/issues/98 for details.

I've posted an upstream fix https://github.com/rustls/pki-types/pull/99 but given the severity I think we should defensively patch this here as well.

---

_Label `bug` added by @zanieb on 2026-01-15 22:00_

---

_Marked ready for review by @zanieb on 2026-01-15 22:20_

---

_@konstin approved on 2026-01-16 09:55_

---

_Merged by @zanieb on 2026-01-16 13:12_

---

_Closed by @zanieb on 2026-01-16 13:12_

---
