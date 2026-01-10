```yaml
number: 11885
title: Reconsider use of zlib-rs
type: issue
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
created_at: 2025-03-01T14:33:03Z
updated_at: 2025-03-03T17:29:32Z
url: https://github.com/astral-sh/uv/issues/11885
synced_at: 2026-01-10T03:50:31Z
```

# Reconsider use of zlib-rs

---

_Issue opened by @charliermarsh on 2025-03-01 14:33_

We reverted back to zlib-ng because it does runtime feature detection. It looks like newer versions of zlib-rs do runtime feature detection and the benchmarks are very promising. We should try it again.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-01 14:33_

---

_Label `performance` added by @charliermarsh on 2025-03-01 14:33_

---

_Unassigned @charliermarsh by @charliermarsh on 2025-03-02 01:07_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-02 03:04_

---

_Closed by @charliermarsh on 2025-03-03 17:29_

---

_Closed by @charliermarsh on 2025-03-03 17:29_

---
