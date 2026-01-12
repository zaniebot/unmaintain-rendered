```yaml
number: 17111
title: Drop arm musl caveat from Docker documentation
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/arm-musl
created_at: 2025-12-12T19:11:01Z
updated_at: 2025-12-12T19:15:16Z
url: https://github.com/astral-sh/uv/pull/17111
synced_at: 2026-01-12T16:12:37Z
```

# Drop arm musl caveat from Docker documentation

---

_@zanieb_

This works fine now

```
❯ docker run --rm -it ghcr.io/astral-sh/uv:alpine sh -c "uv python install 3.14"
Installed Python 3.14.2 in 2.77s
 + cpython-3.14.2-linux-aarch64-musl (python3.14)
```

---

_Label `documentation` added by @zanieb on 2025-12-12 19:11_

---

_Merged by @zanieb on 2025-12-12 19:15_

---

_Closed by @zanieb on 2025-12-12 19:15_

---

_Branch deleted on 2025-12-12 19:15_

---
