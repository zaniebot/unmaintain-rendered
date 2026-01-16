```yaml
number: 17536
title: Investigate hash checks for shellcheck installation in CI
type: issue
state: open
author: zanieb
labels:
  - internal
assignees: []
created_at: 2026-01-16T18:49:43Z
updated_at: 2026-01-16T18:49:49Z
url: https://github.com/astral-sh/uv/issues/17536
synced_at: 2026-01-16T19:01:52Z
```

# Investigate hash checks for shellcheck installation in CI

---

_@zanieb_

We should probably check that what we downloaded matches a specified hash, to prevent this being a supply chain attack route. But I'll grant you that the original action didn't do this, so this strictly isn't making things worse.

_Originally posted by @EliteTK in https://github.com/astral-sh/uv/pull/17532#discussion_r2699543921_
            

---

_Label `internal` added by @zanieb on 2026-01-16 18:49_

---
