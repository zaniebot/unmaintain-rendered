```yaml
number: 5804
title: "Use `UrlString` for `Source` struct"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/lock-hash
created_at: 2024-08-05T23:46:33Z
updated_at: 2024-08-06T12:58:10Z
url: https://github.com/astral-sh/uv/pull/5804
synced_at: 2026-01-12T16:07:02Z
```

# Use `UrlString` for `Source` struct

---

_@charliermarsh_

I don't think this will save any time in serialization, but it should save us some deserialization, since we only need to parse URLs for the packages we use...

---

_Label `internal` added by @charliermarsh on 2024-08-05 23:46_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-08-05 23:46_

---

_@konstin approved on 2024-08-06 08:06_

---

_Merged by @charliermarsh on 2024-08-06 12:58_

---

_Closed by @charliermarsh on 2024-08-06 12:58_

---

_Branch deleted on 2024-08-06 12:58_

---
