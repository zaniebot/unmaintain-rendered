```yaml
number: 12694
title: Make CRC checking opt-in
type: issue
state: closed
author: charliermarsh
labels:
  - compatibility
assignees: []
created_at: 2025-04-07T01:26:46Z
updated_at: 2025-04-07T17:49:07Z
url: https://github.com/astral-sh/uv/issues/12694
synced_at: 2026-01-12T16:01:10Z
```

# Make CRC checking opt-in

---

_@charliermarsh_

We need to iron out a few bugs here (#12677). See: https://github.com/astral-sh/uv/issues/12618.

---

_Label `compatibility` added by @charliermarsh on 2025-04-07 01:27_

---

_Assigned to @Gankra by @zanieb on 2025-04-07 01:41_

---

_Comment by @konstin on 2025-04-07 16:51_

Can we check whether data descriptors are used and if so, skip CRC validation until the async-zip library supports data descriptors? The field already exists in `rs-async-zip`: https://github.com/charliermarsh/rs-async-zip/compare/main...konstin:rs-async-zip:konsti/data-descriptor

---

_Comment by @charliermarsh on 2025-04-07 17:03_

Yeah that sounds like it might be a better approach.

---

_Closed by @zanieb on 2025-04-07 17:49_

---

_Closed by @zanieb on 2025-04-07 17:49_

---
