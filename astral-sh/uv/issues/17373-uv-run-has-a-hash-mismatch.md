```yaml
number: 17373
title: uv run has a hash mismatch
type: issue
state: open
author: senn-hermle
labels:
  - bug
assignees: []
created_at: 2026-01-09T09:38:42Z
updated_at: 2026-01-09T12:37:06Z
url: https://github.com/astral-sh/uv/issues/17373
synced_at: 2026-01-12T16:02:49Z
```

# uv run has a hash mismatch

---

_@senn-hermle_

### Summary

I think this is the same problem as #17143 

  × Failed to download `click==8.3.1`
  ╰─▶ Hash mismatch for `click==8.3.1`

      Expected:
        sha256:12ff4785d337a1bb490bb7e9c2b1ee5da3112e94a8622f26a6c77f5d2fc6842a
        sha512:ad24b2750355f4a178dc397c98645065f9ef18e19b5ac90c11013a7a45040593b1e9b9475cf8c42df51a44f21fc63e29af001d4f43461e2d6db4cca5fc26bdfe

      Computed:
        sha256:981153a64e25f12d547d3426c367a4857371575ee7ad18df2a6183ab0545b2a6
  help: `click` (v8.3.1) was included because `sinmdgen` depends on `click`

@charliermarsh ?

### Platform

Windows 11

### Version

 uv 0.9.17 (2b5d65e61 2025-12-09)

### Python version

 3.13.6 

---

_Label `bug` added by @senn-hermle on 2026-01-09 09:38_

---

_Comment by @senn-hermle on 2026-01-09 10:23_

When I delete the cache folder, it works again. But that can’t be the real solution, right?

C:\Users\XXX\AppData\Local\uv\cache

---

_Comment by @zanieb on 2026-01-09 12:37_

What package index are you using?

---
