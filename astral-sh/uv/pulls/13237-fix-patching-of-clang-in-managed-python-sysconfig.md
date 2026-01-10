```yaml
number: 13237
title: "Fix patching of `clang` in managed Python sysconfig"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-patch
created_at: 2025-04-30T17:06:03Z
updated_at: 2025-05-04T12:26:40Z
url: https://github.com/astral-sh/uv/pull/13237
synced_at: 2026-01-10T11:10:41Z
```

# Fix patching of `clang` in managed Python sysconfig

---

_Pull request opened by @zanieb on 2025-04-30 17:06_

Regressed in https://github.com/astral-sh/uv/pull/12239/files#r2069106892 because additional entries override the previous one in the mapping. Now, we can apply multiple patches in-order.

Closes #13236 

---

_Label `bug` added by @zanieb on 2025-04-30 17:06_

---

_Review requested from @charliermarsh by @zanieb on 2025-04-30 17:07_

---

_Review comment by @charliermarsh on `crates/uv-python/src/sysconfig/mod.rs`:299 on 2025-04-30 17:11_

Should we break on first match?

---

_@charliermarsh approved on 2025-04-30 17:11_

---

_@zanieb reviewed on 2025-04-30 17:14_

---

_Review comment by @zanieb on `crates/uv-python/src/sysconfig/mod.rs`:299 on 2025-04-30 17:14_

I'm not sure, it seems like there _could_ be use for repeated patches? I wouldn't assume break-first behavior when declaring the patches.

---

_Merged by @zanieb on 2025-04-30 17:23_

---

_Closed by @zanieb on 2025-04-30 17:23_

---

_Branch deleted on 2025-04-30 17:23_

---

_Comment by @samypr100 on 2025-05-02 00:16_

Thanks for fixing @zanieb 🙌

---
