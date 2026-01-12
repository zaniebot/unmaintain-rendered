```yaml
number: 3124
title: Restore seeding of authentication cache from index URLs
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/seed-cach
created_at: 2024-04-18T17:52:46Z
updated_at: 2024-04-19T00:48:22Z
url: https://github.com/astral-sh/uv/pull/3124
synced_at: 2026-01-12T16:05:26Z
```

# Restore seeding of authentication cache from index URLs

---

_@zanieb_

Roughly reverts https://github.com/astral-sh/uv/pull/2976/commits/f7820ceaa7a24613ce89da77c48a0454c1e94616 to reduce possible race conditions for pre-authenticated index URLs

Part of:

- https://github.com/astral-sh/uv/issues/3123
- https://github.com/astral-sh/uv/issues/3122

---

_Label `bug` added by @zanieb on 2024-04-18 17:52_

---

_Marked ready for review by @zanieb on 2024-04-18 22:44_

---

_@charliermarsh approved on 2024-04-19 00:42_

---

_Merged by @zanieb on 2024-04-19 00:48_

---

_Closed by @zanieb on 2024-04-19 00:48_

---

_Branch deleted on 2024-04-19 00:48_

---
