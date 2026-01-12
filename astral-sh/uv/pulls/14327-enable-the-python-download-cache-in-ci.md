```yaml
number: 14327
title: Enable the Python download cache in CI
type: pull_request
state: open
author: zanieb
labels:
  - internal
assignees: []
draft: true
base: main
head: zb/py-install-cache-ci
created_at: 2025-06-27T19:30:15Z
updated_at: 2025-07-28T17:46:39Z
url: https://github.com/astral-sh/uv/pull/14327
synced_at: 2026-01-12T16:11:09Z
```

# Enable the Python download cache in CI

---

_@zanieb_

Interestingly, this seems to provide no speed-up? The `uv python install` invocation _does_ use the cached versions though, so I think this will achieve the goal of reducing network failures.

Follows https://github.com/astral-sh/uv/pull/14326

---

_Label `internal` added by @zanieb on 2025-06-27 19:30_

---
