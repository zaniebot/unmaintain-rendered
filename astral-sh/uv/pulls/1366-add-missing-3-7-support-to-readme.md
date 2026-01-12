```yaml
number: 1366
title: Add missing 3.7 support to README
type: pull_request
state: closed
author: T-256
labels:
  - documentation
assignees: []
base: main
head: patch-1
created_at: 2024-02-15T21:58:09Z
updated_at: 2024-02-15T22:05:46Z
url: https://github.com/astral-sh/uv/pull/1366
synced_at: 2026-01-12T16:04:36Z
```

# Add missing 3.7 support to README

---

_@T-256_

_No description provided._

---

_Comment by @zanieb on 2024-02-15 21:59_

Thanks for contributing! We're explicitly not advertising 3.7 support since it's EOL. We do some testing against it, but it's not intended to be user-facing support.

---

_Closed by @zanieb on 2024-02-15 21:59_

---

_Label `documentation` added by @zanieb on 2024-02-15 21:59_

---

_Comment by @T-256 on 2024-02-15 22:05_

I just saw few usage examples of it on the blog post, but then wondered why not included in supported list.


> We're explicitly not advertising 3.7 support since it's EOL

How about to mention actual behavior?
```
uv supports and is tested against Python 3.7 (EOL), 3.8, 3.9, 3.10, 3.11, and 3.12.
```

---
