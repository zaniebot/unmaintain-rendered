```yaml
number: 15645
title: Add zstandard support for wheels
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/pyx-zstd
created_at: 2025-09-02T22:05:15Z
updated_at: 2025-09-03T08:01:08Z
url: https://github.com/astral-sh/uv/pull/15645
synced_at: 2026-01-10T06:44:33Z
```

# Add zstandard support for wheels

---

_Pull request opened by @charliermarsh on 2025-09-02 22:05_

## Summary

This PR allows pyx to send down hashes for zstandard-compressed tarballs. If the hash is present, then the file is assumed to be present at `${wheel_url}.tar.zst`, similar in design to PEP 658 `${wheel_metadata}.metadata` files. The intent here is that the index must include the wheel (to support all clients and support random-access), but can optionally include a zstandard-compressed version alongside it, for clients that support it.


---

_Review requested from @zanieb by @charliermarsh on 2025-09-02 22:05_

---

_Review requested from @konstin by @charliermarsh on 2025-09-02 22:05_

---

_Marked ready for review by @charliermarsh on 2025-09-02 22:05_

---

_Label `enhancement` added by @charliermarsh on 2025-09-02 22:05_

---

_@zanieb reviewed on 2025-09-02 22:12_

---

_Review comment by @zanieb on `crates/uv-extract/src/stream.rs`:539 on 2025-09-02 22:12_

Note this comment changed

---

_@zanieb reviewed on 2025-09-02 22:12_

---

_Review comment by @zanieb on `crates/uv-extract/src/stream.rs`:539 on 2025-09-02 22:12_

As did the implementation

---

_@zanieb reviewed on 2025-09-02 22:14_

---

_Review comment by @zanieb on `crates/uv-distribution/src/distribution_database.rs`:1334 on 2025-09-02 22:14_

`#[must_use]`?

---

_@zanieb reviewed on 2025-09-02 22:15_

---

_Review comment by @zanieb on `crates/uv-distribution/src/distribution_database.rs`:208 on 2025-09-02 22:15_

Nit: It's a little funny these are three conditionals instead of, e.g., one returning a tuple

---

_@zanieb approved on 2025-09-02 22:15_

---

_Review comment by @zanieb on `crates/uv-extract/src/stream.rs`:539 on 2025-09-02 22:16_

Conflict with https://github.com/astral-sh/uv/pull/15452

---

_@zanieb reviewed on 2025-09-02 22:16_

---

_Comment by @zanieb on 2025-09-02 22:16_

Approval conditional on https://github.com/astral-sh/uv/pull/15645#discussion_r2317291103 which seems important

---

_Review comment by @charliermarsh on `crates/uv-extract/src/stream.rs`:539 on 2025-09-02 22:17_

Thanks, sorry. Bad rebase!

---

_@charliermarsh reviewed on 2025-09-02 22:17_

---

_@zanieb reviewed on 2025-09-02 22:25_

---

_Review comment by @zanieb on `crates/uv-extract/src/stream.rs`:539 on 2025-09-02 22:25_

Np!

---

_@charliermarsh reviewed on 2025-09-03 01:11_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/distribution_database.rs`:208 on 2025-09-03 01:11_

Added a struct!

---

_Merged by @charliermarsh on 2025-09-03 01:38_

---

_Closed by @charliermarsh on 2025-09-03 01:38_

---

_Branch deleted on 2025-09-03 01:38_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:4632 on 2025-09-03 08:00_

Is the `METADATA` zstd compressed or the whole wheel?

---

_@konstin reviewed on 2025-09-03 08:01_

---
