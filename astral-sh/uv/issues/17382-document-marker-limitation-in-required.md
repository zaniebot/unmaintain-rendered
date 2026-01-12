```yaml
number: 17382
title: Document marker limitation in required-environments
type: issue
state: open
author: michael-o
labels:
  - documentation
assignees: []
created_at: 2026-01-09T16:46:32Z
updated_at: 2026-01-12T09:21:25Z
url: https://github.com/astral-sh/uv/issues/17382
synced_at: 2026-01-12T16:02:50Z
```

# Document marker limitation in required-environments

---

_@michael-o_

Based one https://github.com/astral-sh/uv/issues/17109#issuecomment-3671308485 the documentation shall say that markers are limited to those in the Rust code and cannot be freely used, e.g.,
```
[tool.uv]
required-environments = [
    "sys_platform == 'freebsd13'",
    "sys_platform != 'freebsd13'"
]
```

That could have saved parts of #17109.

---

_Label `documentation` added by @konstin on 2026-01-12 09:21_

---
