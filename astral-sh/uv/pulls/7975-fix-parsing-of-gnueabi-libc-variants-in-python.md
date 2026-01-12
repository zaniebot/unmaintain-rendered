```yaml
number: 7975
title: "Fix parsing of `gnueabi` libc variants in Python version requests"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/gnueabi
created_at: 2024-10-07T15:39:57Z
updated_at: 2024-10-07T15:49:22Z
url: https://github.com/astral-sh/uv/pull/7975
synced_at: 2026-01-12T16:08:06Z
```

# Fix parsing of `gnueabi` libc variants in Python version requests

---

_@zanieb_

```
❯ cargo run -q -- python install cpython-3.12.6-linux-armv7-gnueabihf
Searching for Python versions matching: cpython-3.12.6-linux-armv7-gnueabihf
Installed Python 3.12.6 in 2.10s
 + cpython-3.12.6-linux-armv7-gnueabihf

❯ uv python install cpython-3.12.6-linux-armv7-gnueabihf
error: Cannot download managed Python for request: executable name `cpython-3.12.6-linux-armv7-gnueabihf`
```

---

_Label `bug` added by @zanieb on 2024-10-07 15:39_

---

_@konstin approved on 2024-10-07 15:43_

---

_Merged by @zanieb on 2024-10-07 15:49_

---

_Closed by @zanieb on 2024-10-07 15:49_

---

_Branch deleted on 2024-10-07 15:49_

---
