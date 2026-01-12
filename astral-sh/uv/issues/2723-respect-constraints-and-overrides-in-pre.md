```yaml
number: 2723
title: Respect constraints and overrides in pre-resolution iterators
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-03-28T22:57:37Z
updated_at: 2024-03-31T18:04:06Z
url: https://github.com/astral-sh/uv/issues/2723
synced_at: 2026-01-12T15:58:40Z
```

# Respect constraints and overrides in pre-resolution iterators

---

_@charliermarsh_

Before resolution, we often iterate over our requirements to determine whether (e.g.) we need to allow a URL. These iterators include constraints and overrides, but they put them _on top_ of the requirements, rather than overriding them. This leads to some strange behavior that's fairly rare in practice (I checked in some tests previously to illustrate it), but still should be fixed.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-28 22:57_

---

_Label `internal` added by @charliermarsh on 2024-03-28 22:58_

---

_Comment by @charliermarsh on 2024-03-28 22:58_

Also a requirement for #2684, I think.

---

_Label `internal` removed by @charliermarsh on 2024-03-29 17:50_

---

_Label `bug` added by @charliermarsh on 2024-03-29 17:50_

---

_Comment by @charliermarsh on 2024-03-31 18:04_

Closed by #2742.

---

_Closed by @charliermarsh on 2024-03-31 18:04_

---
