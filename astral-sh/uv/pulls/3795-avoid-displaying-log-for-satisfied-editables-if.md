```yaml
number: 3795
title: Avoid displaying log for satisfied editables if none are requested
type: pull_request
state: merged
author: zanieb
labels:
  - tracing
assignees: []
merged: true
base: main
head: zb/editables
created_at: 2024-05-23T14:35:54Z
updated_at: 2024-05-23T15:07:36Z
url: https://github.com/astral-sh/uv/pull/3795
synced_at: 2026-01-12T16:05:51Z
```

# Avoid displaying log for satisfied editables if none are requested

---

_@zanieb_

e.g. in `uv pip install anyio -v` this message is just noise

```
DEBUG Requirement satisfied: anyio
DEBUG Requirement satisfied: idna>=2.8
DEBUG Requirement satisfied: sniffio>=1.1
DEBUG All editables satisfied: 
```

---

_Label `tracing` added by @zanieb on 2024-05-23 14:35_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip/install.rs`:162 on 2024-05-23 14:53_

Nit: this is missing a period.

---

_@charliermarsh approved on 2024-05-23 14:53_

---

_@zanieb reviewed on 2024-05-23 14:57_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/install.rs`:162 on 2024-05-23 14:57_

```suggestion
    // Check if the current environment satisfies the requirements.
```

---

_@konstin approved on 2024-05-23 14:58_

---

_Merged by @zanieb on 2024-05-23 15:07_

---

_Closed by @zanieb on 2024-05-23 15:07_

---

_Branch deleted on 2024-05-23 15:07_

---
