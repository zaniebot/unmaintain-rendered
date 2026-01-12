```yaml
number: 14703
title: Retry request on invalid data error
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/retry-on-invalid-data
created_at: 2025-07-18T07:23:40Z
updated_at: 2025-07-20T22:28:35Z
url: https://github.com/astral-sh/uv/pull/14703
synced_at: 2026-01-12T16:11:21Z
```

# Retry request on invalid data error

---

_@konstin_

I also improved the trace logging.

Fixes #14699

---

_Label `bug` added by @konstin on 2025-07-18 07:23_

---

_@jtfmumm reviewed on 2025-07-18 09:38_

---

_Review comment by @jtfmumm on `crates/uv-client/src/base_client.rs`:926 on 2025-07-18 09:38_

Could you also do a match here? Or an `if let`, though maybe that would be harder to read 

---

_@jtfmumm approved on 2025-07-18 09:38_

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:926 on 2025-07-18 09:59_

We could, but it should be the same to llvm, it's not performance critical (we hit this branch up to 3 times and only after we've done a network request) and I found it easier to read and format (I added the comments in the `if` originally, but that made Rustfmt give up on the whole block).

---

_@konstin reviewed on 2025-07-18 09:59_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:946 on 2025-07-18 11:35_

Can we only show this last log  if the `for` loop doesn't iterate?

---

_@zanieb reviewed on 2025-07-18 11:35_

---

_Comment by @zanieb on 2025-07-18 11:35_

Thank you!

---

_Merged by @konstin on 2025-07-20 22:28_

---

_Closed by @konstin on 2025-07-20 22:28_

---

_Branch deleted on 2025-07-20 22:28_

---
