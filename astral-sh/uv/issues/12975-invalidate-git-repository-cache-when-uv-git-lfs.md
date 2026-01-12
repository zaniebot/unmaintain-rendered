```yaml
number: 12975
title: "Invalidate Git repository cache when `UV_GIT_LFS` value differs"
type: issue
state: open
author: zanieb
labels:
  - bug
  - cache
assignees: []
created_at: 2025-04-18T21:04:29Z
updated_at: 2025-04-18T21:04:35Z
url: https://github.com/astral-sh/uv/issues/12975
synced_at: 2026-01-12T16:01:16Z
```

# Invalidate Git repository cache when `UV_GIT_LFS` value differs

---

_@zanieb_

We can get into a bad state with Git LFS if the repository is cached

e.g., see 

- #12938

We should at least provide a better error when we see LFS problems? But the broken cache seems fixable.

---

_Label `bug` added by @zanieb on 2025-04-18 21:04_

---

_Label `cache` added by @zanieb on 2025-04-18 21:04_

---
