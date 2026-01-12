```yaml
number: 15037
title: "`extra-build-dependencies` don't respect index pinning"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2025-08-03T00:49:06Z
updated_at: 2025-08-04T21:42:12Z
url: https://github.com/astral-sh/uv/issues/15037
synced_at: 2026-01-12T16:02:02Z
```

# `extra-build-dependencies` don't respect index pinning

---

_@charliermarsh_

It looks like we convert from `uv_distribution_types::Requirement` _back_ to `uv_pep508::Requirement`, which is lossy. We lose the ability to pin to specific indexes, for example.


---

_Label `bug` added by @charliermarsh on 2025-08-03 00:49_

---

_Comment by @charliermarsh on 2025-08-03 00:52_

I think that also means we need separate types for lowered vs. not-yet-lowered wrappers.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-08-03 01:43_

---

_Closed by @charliermarsh on 2025-08-04 21:42_

---
