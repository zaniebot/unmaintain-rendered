```yaml
number: 6559
title: "Store test temporary directories outside of `/tmp`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/test-dir
created_at: 2024-08-23T23:13:59Z
updated_at: 2024-08-24T01:51:34Z
url: https://github.com/astral-sh/uv/pull/6559
synced_at: 2026-01-12T16:07:25Z
```

# Store test temporary directories outside of `/tmp`

---

_@charliermarsh_

## Summary

There's a long comment inline describing the motivation here.


---

_Label `testing` added by @charliermarsh on 2024-08-23 23:14_

---

_@zanieb reviewed on 2024-08-23 23:41_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:181 on 2024-08-23 23:41_

"so that if call"?

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:183 on 2024-08-23 23:42_

```suggestion
        // user provided paths.
```

---

_@zanieb reviewed on 2024-08-23 23:42_

---

_@zanieb reviewed on 2024-08-23 23:42_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:394 on 2024-08-23 23:42_

I presume we can drop this now?

---

_@zanieb reviewed on 2024-08-23 23:45_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:210 on 2024-08-23 23:45_

Can probably drop this hack too?

---

_@charliermarsh reviewed on 2024-08-23 23:47_

---

_Review comment by @charliermarsh on `crates/uv/tests/common/mod.rs`:394 on 2024-08-23 23:47_

Yes sorry.

---

_Review comment by @charliermarsh on `crates/uv/tests/common/mod.rs`:210 on 2024-08-23 23:47_

I think this needs to come in a future PR, since we still canonicalize internally...

---

_@charliermarsh reviewed on 2024-08-23 23:47_

---

_@zanieb reviewed on 2024-08-23 23:49_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:210 on 2024-08-23 23:49_

In theory canonical temporary directory for testing == non-canonical now, right?

---

_Review requested from @zanieb by @charliermarsh on 2024-08-23 23:54_

---

_@zanieb reviewed on 2024-08-24 00:00_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:188 on 2024-08-24 00:00_

Sorry, one more ask here — can we respect `UV_INTERNAL__TEST_DIR` or something so downstream maintainers can change this if they need to?

---

_@zanieb approved on 2024-08-24 00:00_

---

_Merged by @charliermarsh on 2024-08-24 01:51_

---

_Closed by @charliermarsh on 2024-08-24 01:51_

---

_Branch deleted on 2024-08-24 01:51_

---
