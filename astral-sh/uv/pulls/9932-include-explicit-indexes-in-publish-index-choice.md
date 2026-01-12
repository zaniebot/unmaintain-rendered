```yaml
number: 9932
title: Include explicit indexes in publish index choice
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/find-explicit-indexes-too
created_at: 2024-12-16T10:28:05Z
updated_at: 2024-12-16T10:40:33Z
url: https://github.com/astral-sh/uv/pull/9932
synced_at: 2026-01-12T16:09:02Z
```

# Include explicit indexes in publish index choice

---

_@konstin_

For publishing, we want to allow all simple `[[tool.uv.index]]` entries, whether they  are explicit or not. We don't allow flat indexes here, assuming that an index you can upload to has a simple index URL (and generally doesn't have a flat index URL, at least I don't know any case that has).

The `no_index` branch isn't used atm, but I left it in case the method gathers more users.

Fixes #9919

---

_Label `bug` added by @konstin on 2024-12-16 10:28_

---

_Review requested from @charliermarsh by @konstin on 2024-12-16 10:28_

---

_Merged by @konstin on 2024-12-16 10:40_

---

_Closed by @konstin on 2024-12-16 10:40_

---

_Branch deleted on 2024-12-16 10:40_

---
