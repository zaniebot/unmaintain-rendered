```yaml
number: 10874
title: "Add test coverage for use of `git` without feature flag"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/disable-git
created_at: 2025-01-22T19:46:04Z
updated_at: 2025-01-23T15:55:09Z
url: https://github.com/astral-sh/uv/pull/10874
synced_at: 2026-01-12T16:09:33Z
```

# Add test coverage for use of `git` without feature flag

---

_@zanieb_

Shoves a broken `git` executable onto the front of the `PATH` in the test context when the `git` feature is disabled so they fail if they're missing the feature-gate.

---

_Label `internal` added by @zanieb on 2025-01-22 19:46_

---

_Renamed from "Test if git is used without flagging" to "Add test coverage for use of `git` without feature flag" by @zanieb on 2025-01-22 20:56_

---

_Marked ready for review by @zanieb on 2025-01-22 21:47_

---

_Review requested from @Gankra by @zanieb on 2025-01-23 05:05_

---

_Review requested from @charliermarsh by @zanieb on 2025-01-23 05:05_

---

_Review comment by @Gankra on `crates/uv/tests/it/common/mod.rs`:475 on 2025-01-23 14:01_

It's funny, this is all *kinds* of weird on windows but that's *fine* because we just need windows to explode (especially because we only need this to break on *some* platform so the CI throws a fit and tells you to add a cfg).

---

_@Gankra approved on 2025-01-23 14:04_

Nice, I gather from all the new attrs that The System Works!

---

_@zanieb reviewed on 2025-01-23 14:06_

---

_Review comment by @zanieb on `crates/uv/tests/it/common/mod.rs`:475 on 2025-01-23 14:06_

Yeah haha it doesn't fail with a nice error message on Windows but it does fail loudly..

---

_@zanieb reviewed on 2025-01-23 14:07_

---

_Review comment by @zanieb on `crates/uv/tests/it/common/mod.rs`:475 on 2025-01-23 14:07_

Is there something better we could do that remains trivial?

---

_@Gankra reviewed on 2025-01-23 15:51_

---

_Review comment by @Gankra on `crates/uv/tests/it/common/mod.rs`:475 on 2025-01-23 15:51_

I don't think so, especially if anything specifically looks for `git.exe` (windows PATH resolution is weird and magic about suffixes and there's two different APIs for looking up things on PATH, it's a whole thing).

---

_@Gankra reviewed on 2025-01-23 15:53_

---

_Review comment by @Gankra on `crates/uv/tests/it/common/mod.rs`:475 on 2025-01-23 15:53_

(Notably the thing JS does to shim binaries onto path doesn't work with rust's Command because of all this, blech)

---

_Merged by @zanieb on 2025-01-23 15:55_

---

_Closed by @zanieb on 2025-01-23 15:55_

---

_Branch deleted on 2025-01-23 15:55_

---
