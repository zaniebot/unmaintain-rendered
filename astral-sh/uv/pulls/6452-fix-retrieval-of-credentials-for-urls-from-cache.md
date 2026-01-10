```yaml
number: 6452
title: Fix retrieval of credentials for URLs from cache
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/auth-url
created_at: 2024-08-22T18:11:52Z
updated_at: 2024-08-23T00:00:59Z
url: https://github.com/astral-sh/uv/pull/6452
synced_at: 2026-01-10T13:09:51Z
```

# Fix retrieval of credentials for URLs from cache

---

_Pull request opened by @zanieb on 2024-08-22 18:11_

While working on https://github.com/astral-sh/uv/pull/6389 I discovered we never checked `cache.get_url` here, which is wrong — though I don't think it had much effect in practice since the realm would typically match first. The main problem is that when we call `get_url` later we hard-code the username to `None` because we assume we checked up here with the username if present. 

---

_Label `bug` added by @zanieb on 2024-08-22 18:11_

---

_Marked ready for review by @zanieb on 2024-08-22 23:07_

---

_@charliermarsh approved on 2024-08-22 23:58_

---

_Merged by @zanieb on 2024-08-23 00:00_

---

_Closed by @zanieb on 2024-08-23 00:00_

---

_Branch deleted on 2024-08-23 00:00_

---
