```yaml
number: 15992
title: "Add `--force` flag for `uv cache clean`"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/cache-force
created_at: 2025-09-22T20:52:54Z
updated_at: 2025-09-22T22:15:15Z
url: https://github.com/astral-sh/uv/pull/15992
synced_at: 2026-01-12T16:12:03Z
```

# Add `--force` flag for `uv cache clean`

---

_@zanieb_

Follows https://github.com/astral-sh/uv/pull/15990 to address concerns there.

---

_Label `enhancement` added by @zanieb on 2025-09-22 20:52_

---

_Renamed from "zb/cache force" to "Add `--force` flag for `uv cache clean`" by @zanieb on 2025-09-22 20:53_

---

_@charliermarsh reviewed on 2025-09-22 21:08_

---

_Review comment by @charliermarsh on `crates/uv-fs/src/lib.rs`:825 on 2025-09-22 21:08_

Should we not propagate the error?

---

_@charliermarsh approved on 2025-09-22 21:08_

---

_@zanieb reviewed on 2025-09-22 21:09_

---

_Review comment by @zanieb on `crates/uv-fs/src/lib.rs`:825 on 2025-09-22 21:09_

We could, but we treat all errors as "the lock could not be acquired" so I don't think the error has semantic meaning to a consumer at this time.

---

_Review comment by @geofft on `crates/uv-fs/src/lib.rs`:716 on 2025-09-22 21:19_

We should loop on `EINTR`, and probably in the blocking cases too, unless I'm missing something that already does that.

---

_@geofft approved on 2025-09-22 21:19_

---

_@zanieb reviewed on 2025-09-22 21:55_

---

_Review comment by @zanieb on `crates/uv-fs/src/lib.rs`:716 on 2025-09-22 21:55_

Deferring to #15996 

---

_Merged by @zanieb on 2025-09-22 22:15_

---

_Closed by @zanieb on 2025-09-22 22:15_

---

_Branch deleted on 2025-09-22 22:15_

---
