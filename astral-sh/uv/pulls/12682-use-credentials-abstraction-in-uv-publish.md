```yaml
number: 12682
title: "Use `Credentials` abstraction in `uv-publish`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - no-build
assignees: []
merged: true
base: main
head: charlie/creds
created_at: 2025-04-04T22:17:20Z
updated_at: 2025-04-07T11:53:22Z
url: https://github.com/astral-sh/uv/pull/12682
synced_at: 2026-01-10T11:10:40Z
```

# Use `Credentials` abstraction in `uv-publish`

---

_Pull request opened by @charliermarsh on 2025-04-04 22:17_

## Summary

I noticed that we aren't using these here -- we have a separate username and password situation.


---

_Review requested from @zanieb by @charliermarsh on 2025-04-04 22:17_

---

_Review requested from @konstin by @charliermarsh on 2025-04-04 22:17_

---

_Label `no-build` added by @charliermarsh on 2025-04-04 22:29_

---

_Label `internal` added by @charliermarsh on 2025-04-04 22:57_

---

_@zanieb approved on 2025-04-04 23:02_

---

_Merged by @charliermarsh on 2025-04-04 23:07_

---

_Closed by @charliermarsh on 2025-04-04 23:07_

---

_Branch deleted on 2025-04-04 23:07_

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:804 on 2025-04-07 11:52_

Do we ever hit this path?

---

_@konstin reviewed on 2025-04-07 11:53_

Thanks!

---
