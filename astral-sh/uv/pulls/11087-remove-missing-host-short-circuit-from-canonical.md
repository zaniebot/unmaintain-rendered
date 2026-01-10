```yaml
number: 11087
title: Remove missing host short-circuit from canonical URL construction
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: zb/remove-host-check
created_at: 2025-01-30T03:42:28Z
updated_at: 2025-01-30T03:45:48Z
url: https://github.com/astral-sh/uv/pull/11087
synced_at: 2026-01-10T11:10:34Z
```

# Remove missing host short-circuit from canonical URL construction

---

_Pull request opened by @zanieb on 2025-01-30 03:42_

This was added in 93d606aba20c39149390a9191879e82194ea4e91 when `Url::set_password` (and username) were unwrapped but now we just ignore those errors (those functions require the URL to have a host).

Removed this to see if it changed any tests and figured I'd commit it, @charliermarsh is poking at it so maybe he's changing behavior that relies on removing this to allow canonicalization of `file://` URLs (which don't have a host).

---

_Review requested from @charliermarsh by @zanieb on 2025-01-30 03:43_

---

_Comment by @zanieb on 2025-01-30 03:45_

Ah it's https://github.com/astral-sh/uv/pull/11088

---

_Closed by @zanieb on 2025-01-30 03:45_

---
