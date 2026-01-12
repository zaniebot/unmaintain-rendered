```yaml
number: 13999
title: "Use fast path in noop with `pip install --group`"
type: issue
state: open
author: konstin
labels:
  - enhancement
  - performance
assignees: []
created_at: 2025-06-12T14:43:57Z
updated_at: 2025-06-12T14:59:47Z
url: https://github.com/astral-sh/uv/issues/13999
synced_at: 2026-01-12T16:01:41Z
```

# Use fast path in noop with `pip install --group`

---

_@konstin_

Using the `--group` option currently disables the fast path for noop installs, which shouldn't be necessary. See https://github.com/astral-sh/uv/pull/13742#discussion_r2142929807

---

_Label `enhancement` added by @konstin on 2025-06-12 14:43_

---

_Label `performance` added by @konstin on 2025-06-12 14:43_

---

_Comment by @Gankra on 2025-06-12 14:59_

On the plus side, the new groups impl also bypasses all requires-dist computation for local packages, which should speed things up.

---
