```yaml
number: 16668
title: Skip release builds on Renovate pull requests
type: pull_request
state: open
author: zanieb
labels:
  - internal
assignees: []
base: main
head: zb/no-build-deps
created_at: 2025-11-10T15:35:05Z
updated_at: 2025-11-10T15:41:12Z
url: https://github.com/astral-sh/uv/pull/16668
synced_at: 2026-01-12T16:12:22Z
```

# Skip release builds on Renovate pull requests

---

_@zanieb_

cc @charliermarsh 

---

_Label `internal` added by @zanieb on 2025-11-10 15:35_

---

_@charliermarsh approved on 2025-11-10 15:36_

---

_Comment by @zanieb on 2025-11-10 15:36_

I guess we might want to turn on release builds on merge to main instead so we can still hunt down regressions? I'm not sure.

---

_Comment by @zanieb on 2025-11-10 15:36_

(that seems even more costly though, long-term)

---
