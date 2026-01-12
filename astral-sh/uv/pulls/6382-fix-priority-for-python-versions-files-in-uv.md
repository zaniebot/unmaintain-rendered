```yaml
number: 6382
title: "Fix priority for `.python-versions` files in `uv python install`"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
  - internal
assignees: []
merged: true
base: main
head: zb/fix-version-priotiy
created_at: 2024-08-21T22:03:04Z
updated_at: 2024-08-21T22:17:15Z
url: https://github.com/astral-sh/uv/pull/6382
synced_at: 2026-01-12T16:07:21Z
```

# Fix priority for `.python-versions` files in `uv python install`

---

_@zanieb_

In https://github.com/astral-sh/uv/pull/6359, I accidentally made `uv python install` prefer `.python-version` files over `.python-versions` files -.-, kind of niche but it's a regression.

---

_Label `internal` added by @zanieb on 2024-08-21 22:03_

---

_Label `bug` added by @zanieb on 2024-08-21 22:03_

---

_Marked ready for review by @zanieb on 2024-08-21 22:09_

---

_Merged by @zanieb on 2024-08-21 22:17_

---

_Closed by @zanieb on 2024-08-21 22:17_

---

_Branch deleted on 2024-08-21 22:17_

---
