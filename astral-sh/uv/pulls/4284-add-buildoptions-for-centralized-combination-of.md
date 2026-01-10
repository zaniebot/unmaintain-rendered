```yaml
number: 4284
title: "Add `BuildOptions` for centralized combination of `NoBuild` and `NoBinary`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/build-options
created_at: 2024-06-12T18:05:39Z
updated_at: 2024-06-12T21:33:34Z
url: https://github.com/astral-sh/uv/pull/4284
synced_at: 2026-01-10T13:54:02Z
```

# Add `BuildOptions` for centralized combination of `NoBuild` and `NoBinary`

---

_Pull request opened by @zanieb on 2024-06-12 18:05_

As requested in review of https://github.com/astral-sh/uv/pull/4067

---

_Review requested from @charliermarsh by @zanieb on 2024-06-12 18:07_

---

_Comment by @zanieb on 2024-06-12 18:15_

Ah I broke something..

---

_Review requested from @konstin by @zanieb on 2024-06-12 18:28_

---

_Label `internal` added by @zanieb on 2024-06-12 18:30_

---

_@charliermarsh reviewed on 2024-06-12 20:44_

---

_Review comment by @charliermarsh on `crates/uv-dispatch/src/lib.rs`:325 on 2024-06-12 20:44_

Should this be an `else`?

---

_@charliermarsh reviewed on 2024-06-12 20:44_

---

_Review comment by @charliermarsh on `crates/uv-dispatch/src/lib.rs`:325 on 2024-06-12 20:44_

I think it's unreachable but it wasn't clear to me.

---

_@charliermarsh approved on 2024-06-12 20:45_

Nice, this looks great.

---

_@zanieb reviewed on 2024-06-12 20:46_

---

_Review comment by @zanieb on `crates/uv-dispatch/src/lib.rs`:325 on 2024-06-12 20:46_

I wrote it as an else and the compiler complained that it was redundant, it shouldn't be unreachable.

---

_@zanieb reviewed on 2024-06-12 20:47_

---

_Review comment by @zanieb on `crates/uv-dispatch/src/lib.rs`:325 on 2024-06-12 20:47_

The distribution can be `None` and we won't have a name. It's definitely(??) reachable because I hit this error when I broke the tests :)

---

_@charliermarsh reviewed on 2024-06-12 20:58_

---

_Review comment by @charliermarsh on `crates/uv-dispatch/src/lib.rs`:325 on 2024-06-12 20:58_

I honestly might just write out `return Err` rather than the `bail` to make it clearer.

---

_@zanieb reviewed on 2024-06-12 21:16_

---

_Review comment by @zanieb on `crates/uv-dispatch/src/lib.rs`:325 on 2024-06-12 21:16_

That's fine with me, just copied this from the old code.

---

_Merged by @zanieb on 2024-06-12 21:33_

---

_Closed by @zanieb on 2024-06-12 21:33_

---

_Branch deleted on 2024-06-12 21:33_

---
