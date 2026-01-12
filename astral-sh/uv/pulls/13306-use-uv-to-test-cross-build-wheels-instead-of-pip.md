```yaml
number: 13306
title: Use uv to test cross-build wheels instead of pip
type: pull_request
state: open
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: zb/uv-arch
created_at: 2025-05-05T23:59:54Z
updated_at: 2025-06-08T01:45:24Z
url: https://github.com/astral-sh/uv/pull/13306
synced_at: 2026-01-12T16:10:38Z
```

# Use uv to test cross-build wheels instead of pip

---

_@zanieb_

In an attempt to avoid the horrible cross-build segfault do to apt install failures... use uv instead of pip to perform the installation.

---

_Comment by @zanieb on 2025-05-06 12:53_

Ah we don't have Python for that architecture? Hm.

---
