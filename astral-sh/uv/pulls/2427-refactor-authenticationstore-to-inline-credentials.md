```yaml
number: 2427
title: "Refactor `AuthenticationStore` to inline credentials"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/auth-store-global
created_at: 2024-03-13T20:43:01Z
updated_at: 2024-03-13T22:48:02Z
url: https://github.com/astral-sh/uv/pull/2427
synced_at: 2026-01-12T16:05:02Z
```

# Refactor `AuthenticationStore` to inline credentials

---

_@zanieb_

_No description provided._

---

_Label `internal` added by @zanieb on 2024-03-13 20:43_

---

_@zanieb reviewed on 2024-03-13 20:44_

---

_Review comment by @zanieb on `crates/uv-auth/src/store.rs`:169 on 2024-03-13 20:44_

Added some more test coverage here

---

_Review requested from @charliermarsh by @zanieb on 2024-03-13 20:52_

---

_@charliermarsh reviewed on 2024-03-13 20:52_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/lib.rs`:16 on 2024-03-13 20:52_

Is it possible to do this with `Lazy` instead of `lazy_static`, since we already had that dep elsewhere? (I know `lazy_static!` was introduced in the prior PR.)

---

_@zanieb reviewed on 2024-03-13 21:07_

---

_Review comment by @zanieb on `crates/uv-auth/src/lib.rs`:16 on 2024-03-13 21:07_

Yeah I think so

---

_@zanieb reviewed on 2024-03-13 21:08_

---

_Review comment by @zanieb on `crates/uv-auth/src/lib.rs`:16 on 2024-03-13 21:08_

Done

---

_Merged by @zanieb on 2024-03-13 22:48_

---

_Closed by @zanieb on 2024-03-13 22:48_

---

_Branch deleted on 2024-03-13 22:48_

---
