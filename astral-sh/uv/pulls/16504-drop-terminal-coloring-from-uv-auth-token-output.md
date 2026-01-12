```yaml
number: 16504
title: "Drop terminal coloring from `uv auth token` output"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/color
created_at: 2025-10-29T21:38:50Z
updated_at: 2025-10-30T20:10:08Z
url: https://github.com/astral-sh/uv/pull/16504
synced_at: 2026-01-12T16:12:17Z
```

# Drop terminal coloring from `uv auth token` output

---

_@zanieb_

It's too common to set `FORCE_COLOR` in CI which then breaks consumption of the token.

This is actually specific to `pyx.dev`, as we print passwords without coloring.

---

_Label `bug` added by @zanieb on 2025-10-29 21:38_

---

_Review requested from @charliermarsh by @zanieb on 2025-10-29 21:39_

---

_@charliermarsh approved on 2025-10-29 23:12_

---

_Merged by @charliermarsh on 2025-10-30 20:10_

---

_Closed by @charliermarsh on 2025-10-30 20:10_

---

_Branch deleted on 2025-10-30 20:10_

---
