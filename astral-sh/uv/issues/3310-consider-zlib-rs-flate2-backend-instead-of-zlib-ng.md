```yaml
number: 3310
title: "Consider `zlib-rs` flate2 backend instead of `zlib-ng`"
type: issue
state: closed
author: zanieb
labels:
  - performance
  - internal
assignees: []
created_at: 2024-04-29T14:27:10Z
updated_at: 2024-11-20T00:32:48Z
url: https://github.com/astral-sh/uv/issues/3310
synced_at: 2026-01-12T15:58:42Z
```

# Consider `zlib-rs` flate2 backend instead of `zlib-ng`

---

_@zanieb_

See https://github.com/rust-lang/flate2-rs/releases/tag/1.0.29 and #3295 

---

_Label `performance` added by @zanieb on 2024-04-29 14:27_

---

_Label `internal` added by @zanieb on 2024-04-29 14:27_

---

_Comment by @charliermarsh on 2024-04-29 14:33_

Ohh this would be great. (But low priority.)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-30 16:39_

---

_Comment by @hauntsaninja on 2024-05-06 08:58_

Sounds like zlib-rs is actually 5-10% slower than zlib-ng

---

_Comment by @charliermarsh on 2024-05-06 13:36_

I briefly tried and hit some errors, gonna defer for now.

---

_Unassigned @charliermarsh by @charliermarsh on 2024-05-06 13:36_

---

_Closed by @charliermarsh on 2024-11-20 00:32_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-20 00:32_

---
