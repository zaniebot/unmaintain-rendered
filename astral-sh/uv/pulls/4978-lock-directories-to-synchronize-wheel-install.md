```yaml
number: 4978
title: Lock directories to synchronize wheel-install copies
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/locks
created_at: 2024-07-10T21:49:39Z
updated_at: 2024-07-12T00:53:21Z
url: https://github.com/astral-sh/uv/pull/4978
synced_at: 2026-01-10T13:42:52Z
```

# Lock directories to synchronize wheel-install copies

---

_Pull request opened by @charliermarsh on 2024-07-10 21:49_

## Summary

Closes https://github.com/astral-sh/uv/issues/4831.


---

_Label `bug` added by @charliermarsh on 2024-07-10 21:51_

---

_Review requested from @konstin by @charliermarsh on 2024-07-10 21:51_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/linker.rs`:563 on 2024-07-10 21:52_

@konstin - did you have some idea of only using a lock per _top-level_ directory here? Why do that?

---

_@charliermarsh reviewed on 2024-07-10 21:52_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-07-10 21:53_

---

_@ibraheemdev reviewed on 2024-07-10 22:00_

---

_Review comment by @ibraheemdev on `crates/install-wheel-rs/src/linker.rs`:31 on 2024-07-10 22:00_

Looks like the outer `Arc` isn't necessary here.

This also looks very similar to the structure in `uv-distribution`, wonder if we could reuse one of them.

---

_@ibraheemdev approved on 2024-07-10 22:00_

---

_@zanieb reviewed on 2024-07-11 04:03_

---

_Review comment by @zanieb on `crates/install-wheel-rs/src/linker.rs`:563 on 2024-07-11 04:03_

I think the idea is to hold the lock longer?

> We keep that lock until we have a new directory, so when the next files are `foo/bar/b.py` and `foo/baz/c.py`, we already have the lock and it's just a prefix check.

---

_@konstin reviewed on 2024-07-11 07:37_

---

_Review comment by @konstin on `crates/install-wheel-rs/src/linker.rs`:563 on 2024-07-11 07:37_

Yeah, it was meant as a micro-optimization

---

_@konstin approved on 2024-07-11 07:38_

---

_Merged by @charliermarsh on 2024-07-12 00:53_

---

_Closed by @charliermarsh on 2024-07-12 00:53_

---

_Branch deleted on 2024-07-12 00:53_

---
