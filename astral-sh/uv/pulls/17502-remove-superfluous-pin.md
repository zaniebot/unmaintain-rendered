```yaml
number: 17502
title: Remove superfluous pin
type: pull_request
state: open
author: zanieb
labels:
  - internal
assignees: []
draft: true
base: main
head: zb/extra-pin
created_at: 2026-01-15T21:41:15Z
updated_at: 2026-01-15T21:42:16Z
url: https://github.com/astral-sh/uv/pull/17502
synced_at: 2026-01-15T22:02:19Z
```

# Remove superfluous pin

---

_@zanieb_

You can see in https://github.com/astral-sh/uv/pull/3987#discussion_r1624696545 that previously we called `Notified::enable` which required `pin!` but we no longer do that and thus no longer require `pin!`.

---

_Label `internal` added by @zanieb on 2026-01-15 21:41_

---

_Review requested from @ibraheemdev by @zanieb on 2026-01-15 21:42_

---

_Review requested from @oconnor663 by @zanieb on 2026-01-15 21:42_

---
