```yaml
number: 13936
title: Filter managed Python distributions by platform before querying when included in request
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/key-discover
created_at: 2025-06-09T22:43:48Z
updated_at: 2025-06-13T13:50:46Z
url: https://github.com/astral-sh/uv/pull/13936
synced_at: 2026-01-12T16:10:56Z
```

# Filter managed Python distributions by platform before querying when included in request

---

_@zanieb_

In the case where we have platform information in a Python request, we should filter managed Python distributions by it prior to querying them.

Closes https://github.com/astral-sh/uv/issues/13935

---

_Marked ready for review by @zanieb on 2025-06-11 14:18_

---

_Review requested from @Gankra by @zanieb on 2025-06-11 14:18_

---

_Assigned to @Gankra by @zanieb on 2025-06-11 14:18_

---

_Review comment by @Gankra on `crates/uv-python/src/discovery.rs`:643 on 2025-06-12 17:45_

```suggestion
/// The [`PlatformRequest`] is currently only applied to managed Python installations before querying
```

---

_Review comment by @Gankra on `crates/uv-python/src/discovery.rs`:644 on 2025-06-12 17:49_

What does "the caller is responsible" mean?

---

_Review comment by @Gankra on `crates/uv-python/src/discovery.rs`:425 on 2025-06-12 17:49_

Sounds like with this change it also impacts correctness?

---

_@Gankra approved on 2025-06-12 18:01_

Seems to make sense

---

_@zanieb reviewed on 2025-06-12 18:10_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:644 on 2025-06-12 18:10_

If you call this function, you cannot assume that `PlatformRequest` is enforced

---

_@zanieb reviewed on 2025-06-12 18:10_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:644 on 2025-06-12 18:10_

(this is common for pre-filtering operations in this module)

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:425 on 2025-06-12 18:12_

Eh, it prevents us from querying an interpreter that is likely to fail to run on the current platform, but I don't really intend for that to be a responsibility of this function. It's a nice side-effect that this pull request makes that unlikely, but it's more about performance.

---

_@zanieb reviewed on 2025-06-12 18:12_

---

_Merged by @zanieb on 2025-06-13 13:50_

---

_Closed by @zanieb on 2025-06-13 13:50_

---

_Branch deleted on 2025-06-13 13:50_

---
