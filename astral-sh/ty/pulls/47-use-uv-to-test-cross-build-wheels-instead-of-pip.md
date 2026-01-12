```yaml
number: 47
title: Use uv to test cross-build wheels instead of pip
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: zb/uv-arch
created_at: 2025-05-05T23:58:58Z
updated_at: 2025-06-30T22:20:04Z
url: https://github.com/astral-sh/ty/pull/47
synced_at: 2026-01-12T15:54:27Z
```

# Use uv to test cross-build wheels instead of pip

---

_@zanieb_

In an attempt to avoid the horrible cross-build segfault do to apt install failures... use uv instead of pip to perform the installation.

xref https://github.com/astral-sh/uv/pull/13306

---

_Closed by @zanieb on 2025-06-30 22:20_

---
