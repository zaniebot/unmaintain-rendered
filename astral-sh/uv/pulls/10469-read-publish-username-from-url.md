```yaml
number: 10469
title: Read publish username from URL
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/publish-username
created_at: 2025-01-10T12:22:47Z
updated_at: 2025-01-10T20:10:56Z
url: https://github.com/astral-sh/uv/pull/10469
synced_at: 2026-01-10T11:44:51Z
```

# Read publish username from URL

---

_Pull request opened by @konstin on 2025-01-10 12:22_

Setting the username in the URL was possible before, but google cloud artifacts would return a URL redirect for it to the URL without the username in it, and publish (POST) doesn't support URL redirect since the credentials get dropped on redirect.

Removing the username from the URL and setting it as regular username fixes this.

See #9778

---

_Review requested from @zanieb by @konstin on 2025-01-10 12:22_

---

_@zanieb approved on 2025-01-10 17:54_

---

_Merged by @konstin on 2025-01-10 20:10_

---

_Closed by @konstin on 2025-01-10 20:10_

---

_Branch deleted on 2025-01-10 20:10_

---
