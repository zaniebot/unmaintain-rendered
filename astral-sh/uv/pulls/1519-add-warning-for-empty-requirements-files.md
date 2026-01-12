```yaml
number: 1519
title: Add warning for empty requirements files
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
  - tracing
assignees: []
merged: true
base: main
head: zb/warn-empty-requirements
created_at: 2024-02-16T17:37:00Z
updated_at: 2024-02-16T18:28:39Z
url: https://github.com/astral-sh/uv/pull/1519
synced_at: 2026-01-12T16:04:38Z
```

# Add warning for empty requirements files

---

_@zanieb_

Also, improve tracing of requirements file parsing.

Per my confusion in #1334


---

_Label `tracing` added by @zanieb on 2024-02-16 17:37_

---

_Label `error messages` added by @zanieb on 2024-02-16 17:37_

---

_Review requested from @charliermarsh by @zanieb on 2024-02-16 17:49_

---

_@charliermarsh reviewed on 2024-02-16 18:10_

---

_Review comment by @charliermarsh on `crates/uv/src/requirements.rs`:88 on 2024-02-16 18:10_

`path.display()`? Or what type is this?

---

_@charliermarsh approved on 2024-02-16 18:10_

---

_Merged by @zanieb on 2024-02-16 18:19_

---

_Closed by @zanieb on 2024-02-16 18:19_

---

_Branch deleted on 2024-02-16 18:19_

---

_@charliermarsh reviewed on 2024-02-16 18:25_

---

_Review comment by @charliermarsh on `crates/uv/src/requirements.rs`:88 on 2024-02-16 18:25_

Oops, it should actually be `.normalized_display`

---

_@zanieb reviewed on 2024-02-16 18:28_

---

_Review comment by @zanieb on `crates/uv/src/requirements.rs`:88 on 2024-02-16 18:28_

Ugh lol

---

_@zanieb reviewed on 2024-02-16 18:28_

---

_Review comment by @zanieb on `crates/uv/src/requirements.rs`:88 on 2024-02-16 18:28_

What's different? Windows?

---
